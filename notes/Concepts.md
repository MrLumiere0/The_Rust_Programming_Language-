# Macros

In Rust, a macro is a way of writing code that writes other code, known as metaprogramming. Unlike standard functions that take runtime values, macros operate on Rust source code at compile time, expanding into full Rust code before your program finishes compiling. [1] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14), [2] (https://medium.com/@phoomparin/a-beginners-guide-to-rust-macros-5c75594498f1), [3] (https://www.jbinternational.co.uk/article/view/1348)
You can always spot a macro invocation by the exclamation mark (!) at the end of its name, such as println!() or vec![]. [1] (https://www.youtube.com/watch?v=y_hxanYf3cU), [2] (https://medium.com/@nmpiash/rust-declarative-macros-a-practical-guide-to-macro-rules-3ece2e0e064e)

🔑 Why Use Macros Instead of Functions?

• Variable Arguments: Functions must have a fixed number of parameters. Macros can take an infinite number of arguments (like println!("a", b, c, d)).
• Compile-Time Execution: Because they expand at compile time, macros can perform optimizations or code-generation that incurs zero performance penalty at runtime. [1] (https://www.youtube.com/watch?v=Zmoy65pcHlk&t=16), [2] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14)
• Syntax Extension: Macros let you create custom syntax or internal Domain-Specific Languages (DSLs) to reduce boilerplate code. [1] (https://medium.com/@phoomparin/a-beginners-guide-to-rust-macros-5c75594498f1), [2] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14)

🛠️ The Two Main Types of Macros

Rust splits macros into two major categories: Declarative Macros and Procedural Macros. [1] (https://www.jbinternational.co.uk/article/view/1348), [2] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14)

Macro Type	How it Works	Best Used For
Declarative (macro_rules!)	Uses structural pattern matching to replace code blocks.	Simple code shorthand, repeating code variants, and variable arguments.
Procedural	Acts like a compiler plugin to parse and rewrite Rust's abstract syntax tree (AST).	Advanced code generation, custom attributes, and automatic trait implementation.


1. Declarative Macros (macro_rules!)

Declarative macros act like a match expression, but for code syntax. They compare the code you pass them against defined patterns and expand them into new code blocks. [1] (https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/book/second-edition/appendix-04-macros.html), [2] (https://blog.rheinwerk-computing.com/an-introduction-to-macros-in-rust)
Here is a practical breakdown of how a declarative macro works:
rust
// 1. We define a macro using macro_rules!
macro_rules! print_double {
    // 2. The left side is a pattern matcher.
    // '$val' is a variable name, and ':expr' means it must match a valid Rust expression.
    ($val:expr) => {
        // 3. The right side is what the code expands into.
        println!("The double of {} is {}", stringify!($val), $val * 2);
    };
}

fn main() {
    // 4. Invoking the macro
    print_double!(5 + 5); 
}
Use code with caution.

How the compiler processes this:

When you write print_double!(5 + 5);, the compiler captures 5 + 5 as $val. Before compilation finishes, it literally substitutes your code into the pattern, rewriting it as: [1] (https://doc.rust-lang.org/book/ch20-05-macros.html), [2] (https://medium.com/@nmpiash/rust-declarative-macros-a-practical-guide-to-macro-rules-3ece2e0e064e)
rust
println!("The double of {} is {}", stringify!(5 + 5), (5 + 5) * 2);
Use code with caution.

Common Fragment Specifiers

When matching arguments on the left side of a declarative macro, you specify what kind of syntax you expect: [1] (https://oneuptime.com/blog/post/2026-02-03-rust-macros/view)
• :expr – Any expression (e.g., 2 + 2, foo(), y).
• :ident – An identifier, like a variable or function name.
• :ty – A data type (e.g., i32, Vec<String>).
• :path – A namespace path (e.g., std::collections::HashMap). [1] (https://medium.com/@nmpiash/rust-declarative-macros-a-practical-guide-to-macro-rules-3ece2e0e064e)

2. Procedural Macros

Procedural macros function like mini-programs written inside a separate, dedicated crate. They take a stream of Rust tokens as input, manipulate those tokens mathematically/programmatically, and spit out a completely fresh stream of tokens. [1] (https://www.youtube.com/watch?v=Zmoy65pcHlk&t=16), [2] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14), [3] (https://medium.com/@nmpiash/rust-declarative-macros-a-practical-guide-to-macro-rules-3ece2e0e064e)
There are three types of procedural macros: [1] (https://www.youtube.com/watch?v=Zmoy65pcHlk&t=16), [2] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14)

A. Custom Derive Macros

Automatically implements traits for structs or enums. This prevents you from manually writing tedious implementation blocks. [1] (https://www.freecodecamp.org/news/procedural-macros-in-rust/), [2] (https://www.youtube.com/watch?v=Zmoy65pcHlk&t=16), [3] (https://medium.com/@nmpiash/rust-declarative-macros-a-practical-guide-to-macro-rules-3ece2e0e064e)
rust
#[derive(Serialize, Deserialize, Debug)] // Automatically implements these traits
struct User {
    username: String,
    id: u64,
}
Use code with caution.

B. Attribute Macros

Creates custom outer attributes that can transform any code block, including functions or entire structures. [1] (https://www.youtube.com/watch?v=Zmoy65pcHlk&t=16), [2] (https://www.freecodecamp.org/news/procedural-macros-in-rust/)
rust
#[route(GET, "/profile")] // Custom attribute macro that intercepts this function
fn get_profile() {
    // web server logic...
}
Use code with caution.

C. Function-like Procedural Macros

Invoked using an exclamation mark (!), but can handle highly complex, custom inner logic that goes way beyond what simple pattern matching allows. [1] (https://www.youtube.com/watch?v=Zmoy65pcHlk&t=16), [2] (https://www.youtube.com/watch?v=Ij20OxPFrCw&t=14)
rust
