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