*Got back into rust again, and realized I have no clue how to actually create an array of strings, so thought it would be a good time to learn data types.*

Every value in rust has a **Data Type**, which tells rust how to process and use that value. Since rust is a *statically typed language* the program needs to know the data type of every variable at runtime. Most times, the compiler can infer what data type a variable needs to be.

```rust
let x = 15;
/* By default, x will be unsigned int */
let x : i32 = 15;
/* Now x is a 32bit signed Int */
```

Where this would be necessary for instance, is when using a [[Rust Functions#Statements and Expressions|Statement]], since the statement will be executed at runtime, the compiler cannot infer the datatype before then, and as such, will throw an error:

```rust
let x = "42".parse().expect("Not a number!");
/* 
```console
error[E0284]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let x = "42".parse().expect("Not a number!");
  |         ^        ----- type must be known at this point
  |
*/
```
*the parse() is a statement that gets executed once the program starts running, so before then, the compiler can't assign x to be a string, because it doesn't know it will be one yet.*

Let's start getting nasty now

## Scalar Types
A scalar type indicates a single value, such can be [[Signed and Unsigned]] int, or:
- Floats
- Booleans
- char *Define them with single quotes `let x : char = 'x';`*

## Compound Types
These are types that represent one or multiple values

### Tuples
A tuple is a group of values that can each have their own types, tuples are fixed in size, and once declared can neither shrink nor grow.

```rust
fn main(){
	let my_tuple : (f32, char, i8) = (4.0, 'z', -4);
}
```

You can *Destroy* tuples (*fancy way of saying accessing it's values*) by assigning it's values to variables:

```rust
fn main(){
	let my_tuple : (f32, char, i8) = (4.0, 'z', -4);

	let (x, y, z) = my_tuple;

	println!("Value of y is: {y}");
}
```

You can also access it's values directly using:

```rust
let x = my_tuple.0;
let y = my_tuple.1;
/*... you get it*/
```
### Arrays
*What I came here for initially*

Arrays in **Rust** also have a fixed length, and all of it's values must be of the same type.

```rust
fn main(){
	let my_arr = [1, 2, 3, 4];
	
	println!("x is {x}, and y is {y}");
	/* x is 1, and y is 3 */
}
```

you can declare an array by either, writing out each one of his values, or initialize it to contain the same number a number of times:

```rust
fn main(){
	let my_arr = [3; 5];
	/* same as writing */
	let my_arr = [3, 3, 3, 3, 3];
}
```

Declaring the data type would look like:

```rust
let my_arr : [i32; 5];
```
*creates an array of size 5 that contains signed 32ints*

See [[Ownership And Borrowing#String Slices|String Slices]] to find out how to reference the values inside the array.

### Struct

If you're familiar with **OOP**, struct work similarly to a class.

```rust
struct User{
	is_active : bool,
	username : String,
	email : String,
}

fn main(){
	let user1 = User {
		is_active : true,
		username : String::from("John"),
		email : String::from("john@email.com"),
	};

	println!(user1.username);
	// John
}
```

You can also complete values if they're the same in two different Structs:

```rust
fn main(){
	//... We defined user1 here ...
	let user2 = User {
		username: String::from("Maria"),
		..user1
	}
}
```
*All the non specified values in user2, will be filled with the values of user1*

You can create a `Struct` with tuples:

```rust
struct Color(i32, i32, i32);

fn main(){
	let black : Color = Color(0, 0, 0);
}
```
*See [[#Tuples]]*

Or create an **unit struct**, which is simply an empty struct, maybe for future expansion, or the most idiotic state machine I've ever seen proposed by chatgpt

```rust
struct Active;
struct Inactive;

fn check_status(status : Active){
	println!("status is Active.");	
}

fn main(){
	let status = Active;
	check_status(status);
}
```
*I mean it's not even a state machine at this point I don't even know what was going on here...*
### Implement
You can assign functions to particular structs, just like adding functions under classes in python

```rust
Struct Rectangle {
	width : u32,
	height : u32,
}

impl Rectangle {
	fn area(&self) -> u32 {
		self.width * self.height
	}
	fn say_hello() {
		println!("This makes litterally no sense but its an example");
	}
}

fn main(){
	let rect : Rectangle = Rectangle {
		width : 30,
		height: 50,
	}

	println!(rect.area());
}
```

Everything in the `impl` scope will be tied to `Rectangle`

## Enums
*I strongly recommend you go to the official rust book [here](https://doc.rust-lang.org/stable/book/ch06-01-defining-an-enum.html) because it gets really messy*

an **Enum** literally, *Enumeration* is a way of saying that a value is one of possible value, for instance, setting a value to either `NORTH`, `SOUTH`, `EAST`, or `WEST`.

to create an Enum

```rust
enum Direction {
	NORTH,
	SOUTH,
	EAST,
	WEST,
}

fn move_to(direction : Direction){
	match direction {
		Direction::NORTH => println!("Going North"),
		Direction::SOUTH => println!("Going South"),
		// ... you get it
	}	
}

fn main(){
	let x : Direction = Direction::NORTH;
	move_to(x);
}
```

One other really cool use of *Enums*, is being able to pass data through it too

```rust
enum User {
	INFO(String, String, i8),
}

fn main(){
	let new_user = User::INFO(
		String::from("Dave"), 
		String::from("dave@mail.com"), 
		38
		);
	get_age(new_user);
}

fn get_age(user : User){
	match user {
		User::INFO(_, _, age) => println!(age),
	}
}
```

>[!faq] I thought match statements match values? not extract them??
>The match statement in rust is very versatile and can be used both, for matching, and *destructuring*.
> If you wanted to match the age, lets say check if the user is 20years old
> ```rust
>match user {
>	User::INFO(_, _, 20) => println!("user is 20");
>} 
>```
> To properly match the value, rust is forced to destructure it, that's why we get to access the value.

You can even pass structs in Enums
```rust
enum Something {
	a_thing { a_value : i32, another_value : String},
}

fn main(){
	let s : Something = Something::a_thing {
		a_value : 14,
		another_value : String::from("I dont know and I dont care"),
	};
}
```
*Told you it gets messy*.

You can of course use `impl` with `Enum`, see [[#Implement]].

# Collection Types
We're going to take a look at collection types, which are types that allow you to handle a collection of data, similar to [[#Arrays]]. 

## Vector Type

A vector, is similar to an Array, except it size can be undefined (*more like not fixed*)

```rust
let v : Vec<i32> = Vec::new();
```

this creates a new empty vector, the `Vec<i32>` here is **necessary** since the vector is empty, the compiler doesn't know the data type at runtime.

You can create a already filled vector using the `vec!` shortcut
```rust
let v = vec![1, 2, 3];
```
*the type specification is not necessary, it's inferred to be `i32` in this case*

### Updating a vector

You can add values to an existing vector using `push`:

```rust
let mut v = vec![1, 2, 3];

v.push(5);
v.push(6432);
```
*of course without forgetting the mut keyword, see [[Ownership And Borrowing]]*

you can remove a value by **index** using `remove`

```rust
v.remove(1);
```

### Referencing

Of course, you'd want to extract the values from your vector, once again this works similarly to **Arrays**

```rust
let v = vec![1, 2, 3, 4];

let second : &i32 = &v[1];

println!(second);
```
once again, see [[Ownership And Borrowing]]


