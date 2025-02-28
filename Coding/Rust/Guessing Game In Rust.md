Here, you'll code your first simple guessing game in rust to learn the **actual** basics of Rust. You'll see, it's kind of a mindfuck at first, but it's way easier than it looks.

create your project using [[Rust Cargo]], not going to go over it again.

and inside the `main.rs` file, add the following:
```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

**And that's it good job u did it :D**
*jk we'll break it down*

## Creating A New Variable

`use std::io`
To be able to read user input, we need the `io` library *Input/Output*. The `io` library comes from the **standard** library, known as `std`. The **standard** library is included with the Rust installation by default, which is why you do not need to declare it as a dependency in the `Cargo.toml` file.

`let mut guess = String::new();`
*what the fuck is happening here*
- the `let` keyword indicates that we're about to create a new variable
- the `mut` keyword indicates that the variable we're creating is `mutable`, which means it can be changed in the future, *opposite of `const`* 
- `guess` is the name of the variable we're creating
- `= String::new();`  creates a new empty instance of a `String` *let me explain* 
`String` is a string type provided with the **standard** library, it is included in the **prelude** which means that it can be called without using the `std::` as it makes the code more readable, other types in the prelude include : `Vec`, `Option`, `Result` (*We'll see them later don't worry for now*).

`::new()` is calling an **associated function** from the `String` type that returns a *new* empty **String**. 
>[!info] Why is it not `.new()` instead if it's a function?
>Basically an **associated function** is a function that is called on the type itself, rather than on an object of the type.
>Calling `String.new()` would not make sense as it would mean that we are calling a `.new()` function on an instance of `String` called `String`...

Here other examples of variables:
```rust
let mut apple = 5; //mutable
let banana = 3; //immutable
```
*note we didn't specify a type, we don't always need to, similar to `gdscript`, but please nolan always do, we'll see it later*

## Receiving A User Input

Since we added the **Input/Output** library `io` from `std::`, we can now read user input from the terminal.
This is what 
```rust
  io::stdin()
        .read_line(&mut guess)
```
These lines do.

>[!info]
>If we hadn't imported the `::io` library up top, we could still call this **associated function** using 
>```rust
>std::io::stdin()
>```

*Now once again, what is going on??*

The `stdin` function returns an instance of `std::io::Stdin`, which is a type that represents a handle to the standard input of your terminal. *basically means it records your keyboard.*
>[!quote] 
>Think of **`stdin`** like a **straw** that you use to drink water from a glass (the glass being your computer's keyboard or terminal). When you use the straw, you’re taking something from the glass (your input).
>
>- **`std::io::Stdin`** is that straw (the **tool** that helps you "drink" or capture input).
>- **`stdin()`** gives you that straw, so you can use it to get input from the glass (or keyboard in this case).

Next, the `.read_line(&mut guess)` calls the `read_line` function on the `io::stdin` instance. The `&` before the `mut guess` indicates that this is a **Reference**, see [[Ownership And Borrowing]]. It allows you to let multiple parts of your code access the same data without having to copy it into memory a bunch of times. Like variables, **references** are **immutable** by default, so `&mut guess` creates a **mutable reference** of `guess`. 

As you can notice, there is still no `;` indicating the end of the line, this is because all of this is one line of code, separated by newlines to make it more readable, the next line is 
`.expect("Failed To Read Line")`, which we'll see right now.

## Handling Potential Failures With Result

As mentioned before, the `read_line` method puts whatever the user enters into the variable we pass it, but it also returns a **Result** value. A **Result** is an **enumeration**, often called **enum**, which is a type that can be one of multiple **States**.
The possible **states** for **Result** are `Ok`, and `Err`. `Ok` means that the operation completed successfully, and inside it is the generated value, while `Err` means that it didn't, and inside it is information on why it failed...

A **Result** type has a built in function `expect()` that crashes the program with the message passed in `()` if the **Result** is of state `Err`, otherwise, the program just keeps on going.

>[!tip]
>*Took me a while to get it so read closely*
>Basically, neither `Err` nor `Ok` contain the user input, `Err` contains info on why it failed, and `Ok` contains the number of bytes taken **by** the user input **in the case of `read_line()`**. No matter how the program handles the error, `read_line()` assigns it's output to `&mut guess`.
>
>This means that if **Result** somehow is in **State** `Err`, `read_line()` still assigns the user input to `&mut guess`. However, since it failed, it won't assign anything.
>
>`read_line()` is the function assigning the user input to the variable, and it just **also** returns a **Result** type

Rust greatly encourages you to handle all possible errors, you could *technically* run this code without the `.expect("whatever")`, but on run time Rust will give you a warning that you did not handle a possible Error.

## String Placeholders

Great fucking functionality, almost better than **python**, rust lets you have string with place holders in 2 *main* different ways:
```rust
let x = 5
let y = 2

println!("x is {x} and y is {y}, thus x + y = {}", x+y);
// x is 5 and y is 2, thus x + y = 7
```
*note on those little examples I don't use the `fn main(){}` but it's just because I'm lazy, the code wouldn't run without it.*

# Generating A Random Number

Game is pretty ass right now, for 2 reasons:
1. Literally has no number to guess
2. You get only one chance, at doing nothing since there's nothing to guess and nothing to check if you guessed correctly nothing *<3*

## Crates

A crate is how rust calls it's dependencies, or modules, or libraries... whatever you call it.
This is where we're going to use the `Cargo.toml` file as most **outside** dependencies, are... outside, and not built into rust. Only the **standard** library `std::` is built into rust. 
*there are about 2 or 3 others but, complicated shit that doesn't really matter*

### Library Crate vs. Executable Crate

This is pretty simple to understand but I think the difference is important, a **Library Crate**
is a **crate** that provides, types, functions or other such things for a Rust program to use. It doesn't compile into a binary itself, it only provides and is comprised of a `lib.rs` file.

An **Executable Crate** is a crate that itself will be executed, it has a `main.rs` file, and will run some code.

## Using a Crate

To use an external crate, first, list it in the dependencies inside `Cargo.toml`:
```toml
[dependencies]
rand = "0.8.5"
```
In our case, we're using the rand crate which lets us generate random numbers. We're specifying version `0.8.5` which means that the version is **at least** `0.8.5`, but **below** `0.9.0`. This is going off the basis that minor changes are made between `0.8 - 0.8.9`, but major ones *code breaking ones even* could be made in more major updates such as `0.9.0`. *This is the case for every crate not just this one that's just how it works*.

next, build your project:
```sh
$ cargo build
```

This will download every non-downloaded crate, as well as fetch every dependencies for that crate from [Cargo.io](cargo.io), which is where **Rustaceans** (*this is how they call the rust users I think it's cute*) share their own opensource crates for users to work with.

The `Cargo.lock` file is created, to ensure that now, your project, will only use the version of the crate you downloaded. So if next week a later version comes out that breaks your code, because of the `Cargo.lock` file, your program will still use the version you downloaded.

To update your cargo crates to the lates versions, use:
```sh
$ cargo update
```
*This will still ignore `0.9.0`, but might update it to `0.8.6` or something. If you wanted to go to `0.9.0`, you'd have to change the version manually in the `Cargo.toml`*

The last thing you'll have to do is of course, call the crate inside your code, so add this following `use std::io;`
```rust
use rand::Rng;
```
*Rng stands for random number generator*

Update your `main.rs` code to look like this:
```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    println!("The secret number is: {secret_number}");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```

>[!faq] Why import `rand::Rng` if we don't use it?
>this is because we're actually using it, just not explicitly, the `rand::thread_rng()` returns a type of **Rng**, that's why it's important to read the docs carefully when using a crate. the `Rng` type has a function that is `.gen_range()` which is why we can call it.

*You can understand the rest simply tbh*

# Comparing the guess to the secret number

This is the second part of actually making the game, the comparison.

for that we can use another module `cmp` inside the **standard** library, more specifically, we need a type from that library as well called `Ordering`:
```rust
use std::cmp::Ordering
```

Here is the modified code:
```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--

    println!("You guessed: {guess}");

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```
*Note, the trailing comma at the last match arm is not necessary, but it doesn't change anything, just makes the code easier to modify later on so good habit, but ugly*


first off, we use a `match` statement, you know by now but `match` is the equivalent of the one in python, a `case` in C, or a `switch` in gdscript.
*it simply matches the output of what we give it, with different expressions each expression is called an **Arm**, if they match, it executes whatever the statement indicates*

In this case, we call `.cmp(&secret_number)` on our `guess` variable, this means that we're **comparing** (`cmp`) `guess` and `&secret_number`, the `cmp` function returns a `Ordering` type, which is why we imported it earlier.

**Ordering** is also an **enum** type like **Result**, it has 3 possible values:
- `Less` - a < b
- `Greater` - a > b
- `Equal` - a == b

*you get what's happening right*

**However**, the code won't compile, and that's for the simple reason that our user input is of type **String**, but our `secret_number` is of type **i32** (**signed 32bit int**) see [[Signed and Unsigned]].
*Rust defaults int to i32, that's why even without specifying our `random_number` is i32*

What we want to do is convert the int to a string, 
```rust
let guess : u32 = guess
					.trim()
					.parse()
					.expect("Please enter a positive number!");
```
*Why is it that long??? let me show u <3*

firstly the `trim()` removes all spaces and/or newlines from the string. *when pressing enter, it creates a new line so `\n`*

secondly, the `parse()` turns a string into it's target type, in our case we specified that `guess` should be a `u32` using `: u32`, so parse will **try** to turn the string into a `u32`.

lastly, the expect, `parse()`, also returns a **Result** enum, so we can add the same error handling as we did before with the input.

# Adding a Loop

for now our code only runs once, so play once, fail, eat shit. We don't want that, we want our code to run for as long as the user fails.

```rust

	//snip
	loop {
		println!("Please input your guess");
		//snip
		match guess.cmp(&secret_number){
			Ordering::Less => println!("Higher!"),
			Ordering::Greated => println!("Lower!"),
			Ordering::Equal => {
				println!("You win dipshit");
				break;
			},
		}
		
	}
```

Now the code loops the part inside `loop{}` forever, or until the `break;` is called.

# Handling invalid input

For now, every time the user inputs a non number, the game crashes, it's annoying.
What we want, it for the code to disregard the invalid input, repeat the loop.

what we can do is, when the parsing happens, instead use another `match` statement:
```rust
	//snip
	let guess : u32 = match guess.trim().parse() {
		Ok(num) => num,
		Err(_) => continue,
	};

```
*note, the `match` statement here **DOES NOT** end with a semicolon, the semicolon is only here because it is ending the `let` statement, not the `match`*

Since the parse method has a **Result** enum, it's `Ok` state contains the parsed value so return the parsed value, otherwise `continue`, which stops the loop here and runs it again.

>[!info] READ
>Might seem confusing, since earlier we saw that the `Ok` of `io::stdin().read_line()` did not return the string itself, but rather it's length in bytes.
>
>This is normal. The **Result** states are always `Ok` and `Err`, however the value that they contain vary from method to method. In the case of `parse()`, `Ok` contains the parsed number

Here's the entire code now:

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

And this works, good 🥰.
