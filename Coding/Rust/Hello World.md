
This will walk you through creating your first `Hello World` program with rust, you'll learn to create and manage projects as well as printing to the command line.

# Creating The Project

I recommend **NOLAN** that you keep good habits and create a folder for each project, no matter how small, not like you do in python.

The simplest of Rust projects is simply a `.rs` file, that *since you created a folder for each project*, should be called `main.rs`.

Here's the Hello World project's `main.rs` file:
```rust
fn main() { 
	println!("Hello, world!"); 
}
```

## Running your project

To make your `main.rs` file into an executable you can run is similar to using `gcc`

run 
```sh
$ rustc main.rs
$ .\main.exe
```

`rustc` compiles your code, and the second line just runs it *on windows*

>[!tip] 
>Keep in mind that this is the simplest way of creating a Rust project, next you'll learn about [[Rust Cargo]] which you should use **ALL THE TIME**.

# Anatomy Of A Rust Program

Rust is closely tied to **C** and thus works in the same way when it comes to the `fn main()`
```rust
fn main(){

}
```
This defines the `main` function, which is run by default by rust, this is where your *main* code should be.

```rust
	println!("Hello, World!");
```
Firstly, this is the main piece of the program, as it prints the text `Hello, World!` to the terminal. **Rust indentation isn't a TAB it's 4 spaces.**

Secondly, the `!` means that you're calling a **Rust Macro**, and not a function, we'll see Rust Macros later, but for now just know that a `!` means you're calling a **Macro**.

Thirdly, the end of the line is of course marked with the `;`.