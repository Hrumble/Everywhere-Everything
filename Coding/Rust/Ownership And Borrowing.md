Rust has it's own unique Ownership and Borrowing system, which is what makes it's strength.
of course it has similar capacities as pointers and references in C or other similar languages, but here it's even more pushed.
*This is really important  and you have to spend time on it, cause otherwise there's no point coding in rust*

Rust [[Memory Management|stores memory]] on the [[Stack and Heap]] as other program do. However, the way it does is mostly unique to it.

Let's start by taking a look at the **3 Ownership rules**:
- Each value has **one** owner
- There can **only** be one owner at a time
- if the owner goes **out of scope** then the value will be dropped.

# Variable Scope

let's create a simple program to study how variable scope works
```rust
fn main() {    // x is not yet valid here as it has not been declared
	let x : u32 = 4; // x is declared
	// x is valid here
} // Function ends, x no longer exists
```

To further understand this check out [[Stack and Heap#Stack|The Stack]] which explains why that happens and how it happens. 

`x` here is a **local** variable, it is local to the `main` function and does not exist beyond that.

*so far this is the same in all other languages.*

You can create scopes anywhere, by just opening and closing curly braces:

```rust 
fn main(){
	let x : i32 = 1
	// x exists here
	{
		let y : i32 = 2;
		// y exists here
		// x exists here
	}

	// y does not exist here
	// x exists here
}

// nothing exists
```

*The doc introduces the String type here to further explain scopes, so I will do the same thing.*

# The `String` Type

So far the [[Rust Data Types|Data Types]] we've seen are of fixed size and can be stored on the [[Stack and Heap|Stack]], However, we're going to look at a data type that is stored on the heap, and see how and when rust knows to garbage collect

We'll focus mainly on the part of strings that relate to ownership, but these apply to other complex data types, either included or created by you.

So far we've seen what are called **string literals**, which are statically declared strings:

```rust
let x = "this is a static string";
```

Those are stored on the stack and are immutable. Rust has a second string type, which is... `String`, this type manages data allocated on the heap, and as such can store an amount of text unknown at runtime. You can create a `String` from a literal like so:

```rust
let x = String::from("Hello!");
```
*we'll see the :: at a later time i don't know what it is*

You can append a literal string to a **String** using the push_str function:
```rust
x.push_str(" Fuck You");
// Hello! Fuck You
```

So what's the deal what are we getting at here??

See the `String` data type contains 3 bytes of information, a **pointer** which points to the location of the string in memory (heap), a **length** which precises the length of the string in bytes, so 5 letters, len = 5. And a **capacity**, which is the capacity in bytes the string can hold.

**Moving** a simple data type normally happens like the following

```rust
let x = 3; // x is 3
let mut y = x; // y is 3, and so is x, both x and y are 3

y += 5; // y is 8 and x is still 3
```

This is because moving static datatypes simply creates a copy of them, but doing the same with a string, we'd get instead of a copy, a new `String` pointing to the same **pointer** as the previous string, this is **ownership**.

```rust
let s1 = String::from("hello");
let s2 = x;
```

![[pointer_copy_string_rust.svg]]

The issue with this, is that when either `s1` or `s2` will go out of scope, they will both try to free the same memory, which can lead to either double freeing (*issue*), or just unintended behavior.

```rust
let s1 = String::from("hello");
{
	let s2 = s1;
} // s2 frees the memory due to end of scope

println!("{s1}"); // s1 does not exist anymore, it's memory's been freed!
```

```console
error[E0382]: borrow of moved value: `s1`
 --> src/main.rs:5:15
  |
2 |     let s1 = String::from("hello");
  |         -- move occurs because `s1` has type `String`, which does not implement the `Copy` trait
3 |     let s2 = s1;
  |              -- value moved here
4 |
```

To prevent stupid things like that, when assigning an existing complex data type to a new variable, rust invalidates the first variable, rendering it unusable.
In this example, we say that s1 has been **moved** to s2. s1 does not exist anymore.

We can differentiate between **Deep Copy** and **Shallow Copy**, a deep copy copied also the memory in the heap, when doing
```rust
let x = 3;
let y = x;
```

`y` is a **Deep Copy** of `x`. 

A **shallow copy** on the other hand, simply copies the pointer info, so it points to the same allocated memory. However due to rust implementing failsafe measures, meaning the previous variable can't free the memory anymore, we call it a **Move**

>[!info]
> Rust will never create a deep copy by default *excluding simple data types*, so any type of *copying*  can be assumed to have a low impact on performance, **by default**.

**The inverse of this is also true.**
When assigning a new value to an existing variable, **Rust** will by default drop the previous instance of memory.

```rust
let mut s1 = String::from("hello");
s1 = String::from("Penis"); // rust drops the memory previously storing "hello"
```

## Deep Copying
All cool and neat, but what if you want to make a deep copy.
*first off why? you're an idiot you don't know what you're doing you messed something up.*

You can simply use the `clone` method:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```

![[different_pointers_rust.svg]]
## Passing Function parameters

Passing function parameters works the same way as assignment, the value will be moved or copied accordingly.

```rust
fn main(){
	let s1 = String::from("hello"); // s1 is declared and valid here

	takes_ownership(s1); // s1 is passed into the function and thus is dropped
	// s1 is no longer valid here

	let x = 1;
	copies(x);

	// x is still valied here, because it get's copied and not moved by  default (being an int)
}

fn takes_ownership(a_string : String){
	println!("Oh no, {a_string} doesn't exist anymore!");
}

fn copies(an_int : i32){
	println!("All good, {an_int} is still valid");
}
```

So if you wanted to not lose the value to the function, you'd have to return it:

```rust
fn main(){
	let s1 : String = String::from("Hello");
	let s2 = dont_worry(s1);
}

fn dont_worry(a_string : String) -> String{
	// do something with the string
	a_string // return it
}
```

But this is honestly a pain in the ass, so of course, rust has 

# References

To not lose a complex data type every time you pass it to a function, you use references, which is similar to a pointer, in the way that it will point to a location inside the memory.

```rust
fn main(){
	let s1 : String = String::from("hello");

	a_function(&s1);
	// s1 is still valid here
}
// not here tho ofc, just checking if you're following

fn a_function(s : &String){
	// bla bla bla u get it
}
```

the `&s1` is a pointer to the location in the memory where the string `s1` is stored, `s : &String` indicates that the parameter is expecting a reference to a string, thus a pointer that points to a string.
![[reference_rust.svg]]

