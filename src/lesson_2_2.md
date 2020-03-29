# Data types: Ownership

### What will you learn?

1. Ownership in Rust

### Activity

* Write programs to demonstrate ownership concept in Rust

### Anatomy of code

System memory is a scarce resource. When a program needs to be executed, the operating system kernel loads the program instructions (binary) into memory, and initializes variables by allocating memory space based on the data types defined. Most operating systems do not provide access to the applications to access physical memory directly. Instead they provide applications with a directly addressable memory space which is virtual (also called virtual memory). Virtual memory allows applications to access more memory than is physically present, and the operating systems achieve this magic by swapping out unused memory segments into disk space and vice versa, giving the illusion of unlimited memory to the various applications. (In reality, virtual memory itself is limited by the addressable space, eg in 32-bit processer, there is 4B of virtual memory possible.)

A running program is a process. Each process is allocated certain virtual memory by the operating system, and includes the following: program instructions(binary, read-only area), data (static variables initialized by the programmer), **stack** and **heap**. 

Stack is a **self-managing memory system** and is used when the memory requirements of variables are known upfront (at compile time) such as for primitive data types (eg Integer, char, boolean etc) and for arrays of fixed length. The stack is also used during function invocations. Stack contains a series of frames, and a new frame is added when a function is invoked and released when the function completes. Each stack frame contains the variables, function arguments and other relevant information for function to execute. Each thread in a process also gets its own stack.  

The stack is very fast and allocates and clears memory by itself. One reason for this is that the stack uses *last-in first-out* method where in any new memory allocations occur at the top of the stack (and the OS does not have to search the virtual memory to look up free segments to allocate).
The problem comes when the OS has to allocate extra memory for dynamic data structures (eg dynamic arrays, variable-length strings, linked lists etc) where the size of the data structure is known only at runtime and dynamically changes. Such memory allocations are done on the heap. Unlike the stack, heap is **not** self-managing memory. When memory is allocated , it just sits there until it is manually and explicitly deallocated. To manage this, most high level languages like Java, Python, Ruby, Javascript, C# and Go have a mechanism called **garbage collection (GC)** which performs automated memory management. This allows the programmer to simply declare when a variable is needed but not worry about the gory details of when and from where memory is de-allocated. Languages like C and C++ are not garbage-collected languages, and the programmer has to take on this burden, in order to avoid memory leaks, dangling pointers and similar such issues. It is difficult to debug memory-related problems even for experienced programmers. This is why high-level languages use GC. Garbage collection uses its own algorithms to determine which portions of memory need to be deallocated and when. This addes some inefficiencies and also latency to running processes serving requests (due to program pauses during garbage collection), but these are not severe for most application-related tasks. This however becomes an acute problem while writing systems software and for real-time systems where predictable response measured in nanoseconds is expected. Even though GC is not as efficient as manual-memory management, it significantly improves developer productivity, so high-level languages aimed at writing application software use GC.  

Rust is interesting in this regard. It neither expects the developer to do manual memory management (for deallocation) , nor does it do automated garbage collection. It uses a third approach where memory management is performed using a set of ownership rules that is enforced by compiler. This is a unique , innovative approach.

Rust uses the following three ownership rules:  

* Each value in Rust has a variable that’s called its owner.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.

Let us understand the key concepts associated with ownership with examples:  

### Scope

In program below, the scope associated with the two variables **x** and **y** is the *main ( ) function*. At the end of *main () function*, these two variables go out of scope. The variable **x** is initialized with value **4**, and assigning **x** to **y** *copies* the value of **x** into **y**.    

**x** and **y** being of primitive type integer, are stored on the **stack**.

```
fn main() {
    // Integers copy
    let x = 4;
    let y = x;

    println!("x and y are {}, {}", x, y);
} // x and y go out of scope here

```
Let us now look at modified version of the previous function this time enclosing variable **y** within curly braces.

```
fn main() {
    
    let x = 4;
    {
        let y = x; // Integer copy
    } // y goes out of scope here
    println!("x and y are {}, {}", x, y);
} // x goes out of scope here

```

When the program is compiled , you can see a compiler error stating that **y** cannot be printed as it is not in scope.

When a variable goes out of scope, Rust ensures the memory associated with the variable is released back to pool.

### Stack variables vs Heap variables

In the previous example, we used integers. Integers are stored on the **stack**. Let us now look at a data type that is stored on the **heap** - **String**.

Assignment of one heap-allocated variable to another involves transferring or **moving** the value and **not copying**. In code below, when variable **s1** is assigned to **s2**, the value of **s1** is transferred to **s2**. Hence **s2** *owns* the value now. So, compiler will throw an error while trying to print **s1** as **s1** has moved its value to **s2**.

```
fn main() {
    let s1 = String::from("I'm a heap-allocated data");

    let s2 = s1;

    println!("S1 is {}, and s2 is {}", s1, s2);
}
```

By default stack-allocated variables are copied and heap-allocated variables are moved, in Rust.  

The question is , why does Rust **move** values by default and not **copy** them for heap-storage values?

To understand this, we need to take a closer look at two concepts: how copy for pointer variables (heap-allocated variables) works and how memory deallocation for heap-bound variables works. 

1. When a new string variable **s1** is created by the program, a new memory area is allocated for the string data in heap, and only a pointer to it is stored in the stack. When we create another string variable **s2** and assign **s1** to it, if Rust were to copy data (instead of moving), then there would be another pointer created in the stack corresponding to **s2**, but the pointer would point to the same data in the heap as the **s1** pointer. So, both s1 and s2 would represent two different pointers in the stack, both of which point to a common memory are in the heap. Why does Rust not copy heap data? Reason is to avoid unnecessary duplication of dynamic data whose size is unknown at compile time.

2. We already saw that for stack-bound variables , as soon as a variable goes out of scope, Rust knows how to deallocate the memory associated with it. This is also true for heap-allocated data types like String. In  the example below, both **s1** and **s2** go out of scope at the end of the inner curly braces block. Rust will first try to deallocate memory corresponding to say, s1. What this means is the rust will release the memory area in heap and also release the pointer stored in stack for **s1** variable. Then it will try to do the same for **s2**. At this point it will fail, because the heap area associated with the value is already deallocated. So, this a classic 'double free' memory problem.  And because Rust does not want programmer manual intervention for deallocating memory, the memory ownership rules have been established where for heap-variables, when one variable is assigned to another, the value associated with **s1** is moved to **s2**. Then s1 becomes invalid as its value has moved. So at the end of the curly braces block, there is only one variable whose memory needs to be deallocated, which is s2.   

Got it?

```
fn main() {
    {
    let s1 = String::from("I'm a heap-allocated data");

    let s2 = s1;
    // s1 goes out of scope here
    } // here s2 goes out of scope
    
}
```
However, if the programmer wants to explicity copy heap-variables, Rust provides a **clone()** method.

In program below, value of **s1** is cloned and assigned to **s2**. When we print the pointer associated with **s1** and **s2**, you will see that they show two different memory locations, thus confirming that clone() makes a copy of the data at another heap-segment location.  

```

fn main() {
    let s1 = String::from("I'm a heap-allocated data");

    let s2 = s1.clone();

    println!("S1 is {:p}, and s2 is {:p}", s1.as_ptr(), s2.as_ptr());
}
```

### Ownership of data in Functions

The same rules of ownership that we discussed for local variables is applicable even in case of function parameters.

In the following code, when **s1** is passed to function ```print_value```, its value is moved to the function, and **s1** is no longer available for the ```println!``` macro. You will see compiler error here.

```
fn main() {
    let s1 = String::from("I'm a heap-allocated data");

    print_value(s1); // here s1 is moved into function print_value

    println!("S1 is {}", s1);
}

fn print_value(val: String) {
    println!("Printing value {} received", val);
}

```
Another scenario is that when a called function returns a value , the calling function takes ownership of that value, as seen in code below:


```
fn main() {
    let s1 = String::from("I'm a heap-allocated data");
    println!("S1 pointer is {:p}", s1.as_ptr());

    let s2 = print_value(s1); // here s1 is moved into function print_value

    println!("S2 pointer is {:p}", s2.as_ptr());
}

fn print_value(val: String) -> String {
    val // the value received is returned back from function
}
```
Here is the terminal output of the program. You will see that s1 and s2 have the same pointer associated with them, proving that the value is moved into the function and moved back to ```main()```.

```
   Compiling playground v0.0.1 (/playground)
    Finished dev [unoptimized + debuginfo] target(s) in 1.06s
     Running `target/debug/playground`

S1 pointer is 0x55dfc4804a40
S2 pointer is 0x55dfc4804a40
```

This in essence is how Rust ownership works.




