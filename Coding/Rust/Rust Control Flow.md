
yes this is the part that teaches you about if statements and shit

# If statements

Rust does have if statements and they're pretty similar to **Python**:
```rust
fn main(){
	let x : u32 = 4;
	
	if x < 5 {
		println!("x is less than 5");
	} else if x > 5 {
		println!("x is more than 5");
	}
	} else if x == 5 {
		println!("x is equal to 5");
	} else {
		println!("what the fuck?");
	}
	// x is less than 5
}
```

You can also have `if` statements inside `let` statements:
```rust
fn main(){
	let y : u32 = 5;
	let x : u32 = if y < 5 {2} else {1};
	
	println!("{x}")
	// 1
}
```
of course you can't have an `if` statement in a `let` statement if the possible values are of different types.

# Loops

A basic rust loop is marked with the `loop{}` and exits the loop only when `break` is called:

```rust
fn main(){
	let mut x : u32 = 5;
	loop {
		x -= 1;
		if x == 0 {
			println!("Done countdown!");
			break;
		}
	}
}
```

you can also return a value and `break` at the same time:

```rust
fn main() {
	let x : u32 = count();
	println!("{x}")
}

fn count() -> u32 {
	let mut x : u32 = 5
	loop{
		x -= 1;
		if x == 0 {
			break 5+5;
		}
	}
}
```
*I know this makes no sense to code, but I had to come up with something wtv*
The code would output `10`

Break by defaults break the innermost loop. However, you can indicate a particular loop using
**Loop Labels**
```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```
*Code is straight pulled from the doc because I can't bother writing something that would actually use this functionality, just know it's there*


## While Loops

Rust has `while` loops:
```rust
fn main(){
	let mut countdown : u32 = 3;
	while countdown != 0{
		println!("{countdown}!");
		countdown -= 1;
	}
	println!("Liftoff!");
	/* 
		3!
		2!
		1!
		Liftoff!
	*/
}
```

## For Loops


Rust has `for` loops, for that you need an array to work with:
```rust
fn main(){
	let a : [u32; 5] = [10, 20, 30, 40, 50];
	for element in a {
		print("Element is {element}")
	}
}
```

## Match Statements

Match statement allow you to match against a multitude of possibilities:

```rust
fn main(){
	let x : u8 = 10;

	match x {
		5 => println!("x is 5"),
		10 => println!("Eat my ass");
	}
	
}
```

>[!warning] Match statements must cover all possibilities.
> Due to this, it's important to use a catch all statement `_`
> ```rust
> match x {
> 	5 => println!("Cool its 5"),
> 	_ => println!("Its not 5"),
> }
>```
> If for some reason, you need the output of the match all, then you can just use a variable name
> ```rust
> match x {
> 	5 => //something,
> 	other => do_something(other);
> }
>```
> Of course, do_something being a correctly defined function somewhere else *do it yourself I'm not your maid*

*See [[Rust Data Types#Enums|Enums]] for more match power*


**All done good.**