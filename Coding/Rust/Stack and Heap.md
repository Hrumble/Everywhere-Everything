

## The RAM Segments

When you first run a program, a part of your available [[Random Access Memory|RAM]] gets divided into **4 Segments**:

The **Code(Text)**, the **Global/static**, the **Stack**, and the **Heap**.
Each of those has it's own function in running your program.
*The available ram is allocated dynamically, there is no fixed size (except the ones fixed by the OS or of course your ram bar itself)*
![[memory_management_snh.png]]
*The global and static data are part of the **Global** segment, and the machine instructions is the **Code***
### The Code

This part is pretty much static, as it is allocated at compile and consists of your code in machine language. Since you can't change the code as your program runs, this part stays static.

### Global/Static
This part of the memory allocation is used to store any global variable. Meaning a variable that does not belong to a certain scope and can be accessed anywhere in your code.

e.g.
```rust
static mut total : u32;

fn main(){
	total = 1000;
	println!(total);
}
```

In this case, `total` is stored inside the **Static** segment
The allocated size of the **Static** segment is allocated dynamically as more global variables get introduced, the values exist for as long as the program does, and just build up.
*the name static refers to the fact that the memory for the variables stored in it are declared only once and persist for the entire duration of the program*

### Stack
The Stack grows dynamically up to a certain point, defined by the OS, it is limited because after it comes **The Heap**.

The Stack segments stores **local variables**, and **function calls**. It grows as functions are called and shrinks when they return, it follows a **LIFO** principle (*Last in first out*).
*it's way easier to show the stack rather than to explain it*

![[stack_representation.png]]
Here you can see the stack in action in **C**. Each function call and it's local variables are stored in what's called a **Stack Frame**, the higher frames cannot access the values of the frames under.

The program starts by executing the `main()` function, which starts by declaring 2 variables,
`int a = 4; int b = 8`, so the stack stores the values for `a` and `b`.
*The stack does not store the variable names, it does not store the function name, all it stores are the values, which can then be referenced by their memory address.*

Then the second line calls the function `SquareOfSum(a, b);`(*SOS*), so the stack creates a new frame that holds the values for `x`, `y`,  and the newly stated `z`.

It does this a third time for `Square(x+y);` where it only stores the value of the passed `x`. Once this function **Returns**, the stack frame is *bye bye*, goes into oblivion or something idk basically stops existing and the memory is freed (**LIFO**).

*Adding a frame to the stack is called **Pushing** to the stack*

### Heap
The heap grows dynamically, once again it's only limit is set by the OS but it could theoretically occupy all the ram that is needed up until you don't have anymore free ram.

The heap unlike the **Stack** stores variables who's size are undetermined, and are referenceable from anywhere in the code.

The downside of that is 
1. The heap needs to be managed manually in most cases
	1. The allocated memory does not get freed automatically.
2. Accessing data is slower
	1. when using the heap, the OS or memory manager must locate free space, and store metadata about it's location, size and must handle fragmentation.
	2. Since the **Heap**'s memory is non contiguous ("frames" don't follow each other), the data must be referenced with pointers which add extra steps

*By default rust stores everything on the stack, storing on the heap is manual*


