Functions in rust are pretty similar to every other language except for one important difference:
**Statements and Expressions**.

```rust
fn main(){

}
```
you've seen this one before, it's the `main` function, all your code happens here.
to create a function is the same thing, let's create a function that prints Hi:

```rust
fn main(){
	say_hi();
}

fn say_hi(){
	println!("Hi!");
}
```
*pretty simple you get it*

parameters get passed in the same way as any other language too:
```rust
fn main(){
	let x : u32 = 5;
	plus_one(x);
}

fn plus_one(x : u32){
	println!("{x + 1}");
}
```

# Statements and Expressions

Rust distinguishes between a **statement**, does not return anything, simply states.
And an **Expression**, returns a value.

So far what we've seen are statements, `let` is a statement, so is `plus_one()`. However, `parse()`, as seen in [[Guessing Game In Rust#Comparing the guess to the secret number | Guessing Game In Rust]] is an **expression**.

in a `let` statement, the `5` of `let x : u32 = 5` is the expression, just `5`, it returns the value... `5`!
*do you see what I'm getting at? believe me it gets real ugly*

This means that to create an expression (*a function that returns something*), you would do it as such:
```rust
fn main(){
	let x : u32 = 5;
	let y = add_five(x);
}

fn add_five(x : u32) -> u32 {
	x + 5
}
```
Just like that, no semicolon, no `return`, no nothing... *just that*. *adding a semicolon would turn the expression into a statement, and would therefore give us an error as we can't assign x to a statement*

You could also do the same **inside** another function:
```rust
fn main(){
	let x : u32 = {
		let y : u32 = 4;
		y + 5
	};
	println!("{x}")
	// prints 9
}
```
