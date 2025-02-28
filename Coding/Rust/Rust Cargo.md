**Cargo** is Rust's built in project and package manager, **use it**.
It handles your project's dependencies, the compiling, and even creates a `git`.
**Cargo** should come preinstalled if you used the installation methods mentioned in [[Welcome To Rust]], otherwise go fuck yourself.

Check that cargo is installed correctly with:
```sh
$ cargo --version
```

# Creating A Project With Cargo

**Cargo** handles the creation of all the necessary files for you, including a directory for your project so you don't have to manually create it. Create your project using:

```sh
$ cargo new hello_cargo
```

`hello_cargo` being the name of you project and thus the name of the directory. you can `cd` into it to analyze all the created files.

```sh
$ cd hello_cargo
$ ls

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         9/22/2024   5:31 PM                src
-a----         9/22/2024   5:31 PM             81 Cargo.toml

```

As you can see it creates a `src` directory where your `main.rs` file is located, as well as a `Cargo.toml` (*I hate that it uses CamelCase tbh*)

The `Cargo.toml` file contains all your project's dependencies, and rust version, to make it easier for other people to use your project. Somewhat an equivalent to `requirements.txt`

`Cargo.toml`
```toml
[package] 
name = "hello_cargo" 
version = "0.1.0" 
edition = "2021" 
# See more keys and their definitions at https://doc.rust lang.org/cargo/reference/manifest.html 

[dependencies]

```

The first line `[package]` is a **section heading** which indicates that the following statements are configuring a package. As we add more to the project, we'll add other sections.

The last line `[dependencies]`, is the start of the section for you to list your projects dependencies, it's **good habit** to add them as soon as you add them to your project.

Now if you were to open `src/main.rs`, you'll see the following code:
```rust
fn main() { 
	println!("Hello, world!"); 
}
```
Which is the default code in a Rust project.

>[!info]
>Cargo expects all of your code files to be in the `src` directory, the top level directory should be kept for `README` files or other things like this.

# Building and Running the Cargo Project

To create an executable out of your `src` directory, run

```sh
$ cargo build
```

This will create a `binary` executable inside `[...]/target/debug/` with the name of your project, thus `hello_cargo.exe`, as cargo assumes this is just a debug build for now.
You can run this file like you did before, as an executable.

to automatically, build, and run your project, which is way simpler, run:
```sh
$ cargo run
```

This will create the executable and run it all at once.

To simply check your project for errors without building it, run:
```sh
$ cargo check
```

>[!info]
>You'll notice that any `build` command creates a `Cargo.lock` inside the top level directory. This is a locked version of `Cargo.toml` for the current build.

## Building For Release

To build for release instead of debug, run:
```sh
$ cargo build --release
```

This will compile your project with *optimizations* (*no clue what that means*), and create an executable inside of `[...]/target/release`.
