In terminal commands, the **`$`** symbol is primarily used for two main purposes: **shell variables** and **command output substitution**.

Here is a simple breakdown of what it means depending on how it's used:

---

### 1. Accessing Variables (The Most Common Use)

When you put `$` before a name (like `$HOME` or `$USER`), it tells the terminal: **"Replace this with the value stored inside this variable."**

* **`$HOME`** → Resolves to your home folder path (e.g., `/Users/hashid`).
* **`$USER`** → Resolves to your logged-in username (e.g., `hashid`).
* **`$PATH`** → Resolves to the list of folders your terminal searches through to find commands.

#### Examples:

```bash
echo $USER
# Prints: hashid

cd $HOME
# Takes you to /Users/hashid

. "$HOME/.cargo/env"
# Replaces $HOME with /Users/hashid, so it runs: . "/Users/hashid/.cargo/env"

```

---

### 2. Running Command Substitutions `$(...)`

When wrapped around parentheses like `$(command)`, the `$` tells the terminal: **"Run the command inside first, and use its output here."**

#### Examples:

```bash
sudo chown $(whoami) ~/.bash_profile

```

* **`whoami`** outputs your username (`hashid`).
* **`$(whoami)`** grabs that result and plugs it into the command.
* The terminal actually executes: `sudo chown hashid ~/.bash_profile`.

---

### 3. The Terminal Prompt Symbol (Outside of Commands)

Sometimes, in documentation or tutorials, you will see a `$` written at the very beginning of a line:

```bash
$ rustc --version

```

Here, the `$` isn't something you type. It is just a visual placeholder indicating that this is a regular terminal command (as opposed to `#`, which usually indicates a command that must be run as `root` / administrator).

---

### Summary Quick Reference

| Usage | Meaning | Example |
| --- | --- | --- |
| **`$VAR`** | Read a variable's stored text | `echo $PATH` |
| **`$(cmd)`** | Use the output of another command | `echo "I am $(whoami)"` |
| **`$ `** | Visual prompt indicator in tutorials | `$ cargo build` *(don't type the `$`)* |