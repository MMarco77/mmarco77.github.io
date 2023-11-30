# Rust

## Setup rust 💻

1.  Install the [Rust toolchain](https://www.rust-lang.org/tools/install).
2.  (recommended) Install the [rust-analyzer](https://rust-analyzer.github.io/manual.html) extension for your code editor.
3.  (optional) Install a native debugger. If you are using VS Code, [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) is a good option.

## Use VS Code to debug your code

1.  Install [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) and [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb).
2.  Set breakpoints in your code.
3.  Click _Debug_ next to the unit test or the _main_ function.
4.  The debugger will halt your program at the specific line and allow you to inspect the local stack.

## Useful Tools

-   [clippy](https://github.com/rust-lang/rust-clippy): The official Rust linter.
-   [rustfmt](https://github.com/rust-lang/rustfmt): The official Rust code formatter. 

## Useful crates

-   [itertools](https://crates.io/crates/itertools): Extends iterators with extra methods and adaptors. Frequently useful for aoc puzzles.
-   [regex](https://crates.io/crates/regex): Official regular expressions implementation for Rust.;
-   [chrono](https://lib.rs/crates/chrono): The most comprehensive and full-featured datetime library, but more complex because of it.

-   [serde](https://lib.rs/crates/serde): De facto standard serialization library. Use in conjunction with sub-crates like serde_json for the specific format that you are using. 
-   [anyhow](https://lib.rs/crates/anyhow): Provides a boxed error type that can hold any error, and helpers for generating an application-level stack trace.
-   [thiserror](https://lib.rs/crates/thiserror): Helps with generating boilerplate for enum-style error types. 

## Crates

A curated list of popular crates can be found on [blessred.rs](https://blessed.rs/crates).
