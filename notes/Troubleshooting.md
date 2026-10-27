When trying the install curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf |  sh

I got 

error: could not amend shell profile: '/Users/hashid/.bash_profile': could not write rcfile file: '/Users/hashid/.bash_profile': Permission denied (os error 13)

This happens because the installer is trying to update ~/.bash_profile, but your user account doesn't have write permissions for that specific file (likely because it was previously created with root privileges via sudo).

Here is how to fix it quickly:

Step 1: Fix ownership of your profile files
Run this command in your terminal to restore your user's ownership over your configuration files:


sudo chown $(whoami) ~/.bash_profile ~/.zshrc ~/.profile ~/.zshenv ~/.tcshrc 2>/dev/null

Here is a breakdown of what that command does and why it was necessary:

---

### **What the command means (Word-by-Word)**

* **`sudo`**
* **What it means:** "Superuser Do".
* **What it does:** Runs the command with administrative (root) privileges. You need this because if a file belongs to `root`, regular users aren't allowed to change who owns it.


* **`chown`**
* **What it means:** "Change Owner".
* **What it does:** This is the standard Unix command used to change file ownership.


* **`$(whoami)`**
* **What it means:** Evaluates to your current logged-in username (in your case, `hashid`).
* **What it does:** Tells `chown` to set you (`hashid`) as the new owner of the listed files.


* **`~/.bash_profile ~/.zshrc ~/.profile ~/.zshenv ~/.tcshrc`**
* **What it means:** A list of shell configuration files located in your home directory (`~`).
* **What it does:** These are all the files where different shell environments store settings and path variables. The Rust installer tries to write to these files to add Rust to your system command path.


* **`2>/dev/null`**
* **What it means:** Redirects error messages (`2`) to nowhere (`/dev/null`).
* **What it does:** Silences any "File not found" warnings if some of those files (like `.tcshrc` or `.profile`) don't exist on your Mac.



---

### **What Did It Do?**

1. **Reclaimed Ownership:** It changed the file owner of `~/.bash_profile` (and your other shell configuration files) back to **`hashid`** instead of `root`.
2. **Fixed the Permission Error:** Before running this command, `~/.bash_profile` was locked by administrator permissions (likely because a previous script or command was run with `sudo`). Because of that, the Rust installer wasn't allowed to edit it.
3. **Paved the Way for Rustup:** Now, when the Rust installer runs, it can freely append the necessary environment variables to `~/.bash_profile` without throwing the `Permission denied (os error 13)` error.


### Step 2: Rerun the Rustup installer

Now, try running the installer command again:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

```

Press `1` to proceed with the standard installation, and it should complete successfully without permission errors!

Here is a breakdown of what that command means and what it does:

### **What the command means (Word-by-Word)**

* **`curl`**
* **What it means:** A command-line tool used to transfer data to or from a server (effectively downloading something).


* **`--proto '=https'`**
* **What it does:** Forces `curl` to only use the secure HTTPS protocol, blocking any insecure HTTP connections for security.


* **`--tlsv1.2`**
* **What it does:** Ensures that the connection uses at least TLS version 1.2 or higher for secure communication.


* **`[https://sh.rustup.rs](https://sh.rustup.rs)`**
* **What it does:** The web address (URL) of the official Rust installation script hosted by the Rust team.


* **`-sSf`**
* **What it does:** Flags that control `curl`'s output:
* `-s` (silent): Hides the download progress bar.
* `-S` (show error): If something goes wrong, it will still show the error message.
* `-f` (fail): Tells `curl` to fail silently instead of downloading an error page (like a 404 Not Found HTML file) if the server encounters an issue.




* **`|` (The Pipe)**
* **What it does:** Takes the output of the command on the left (downloading the script via `curl`) and sends it directly into the input of the command on the right (running it through the shell).


* **`sh`**
* **What it does:** The Unix shell interpreter that executes the downloaded script line-by-line.



---

### **What Did It Do?**

1. **Downloaded the Installer:** It securely fetched the official Rustup installation script directly from `sh.rustup.rs`.
2. **Executed it Live:** Instead of saving the script as a file on your computer first, the pipe (`|`) handed the script straight to your shell (`sh`) to run immediately.
3. **Set up Rust:** The script checked your operating system (`aarch64-apple-darwin` for Apple Silicon Mac), downloaded the correct Rust compiler (`rustc`) and package manager (`cargo`), and set up your local development environment.



