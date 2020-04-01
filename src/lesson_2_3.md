# Data types: Borrowing & References

### What will you learn?

1. Borrrowing and references in Rust

### Activity

* Write programs to demonstrate ownership concept in Rust.


**Immutable references**

We saw previously that in Rust, when we pass a variable as a parameter to a function, the ownership of the value associated with the variable is moved to the function.  

*What if we don’t want the function to take ownership of the value but just use the value (for whatever purpose)?*

Rust makes allowance for this, and it is called **borrowing**.  

Let’s look at the following code.


```
fn main() {
    // String type
    let s = String::from("Why is Rust so popular?");
    print_sentence(&s);
    println!("In main: {}", s)
} // x goes out of scope here

fn print_sentence(s: &String) {
    println!("In function {}", s);
}

```
The string s is not directly passed to function print_sentence, but a reference to string s is passed instead. What is a reference?  A reference is simply a pointer to the value stored in s. As a result, here the value of s is not moved into the function while invoking it, but only an immutable reference to that value is passed to the function.

Note the function signature of print_sentence. It accepts a parameters that is a reference to a String. When the program runs, you will see the string printed twice, once from within the ```print_sentence``` function and once within the ```main ()``` function. The fact that variable s can be accessed within the ```main()``` function after function invocation shows that the variable **s** is intact and its value is not moved to ```print_sentence``` function.

The *ampersand &* symbol is used to denote a reference, and refers to a value without taking ownership of it.  

*Why are references important?* Without them, everytime a parameter is passed to a function, it needs to be explicitly returned back to the calling function in order to make use of it elsewhere in code.  


**Mutable references**

What if the function wants to modify the value of the input parameter without taking ownership of it? There is a solution for that, and it's called mutable reference. 

```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");
    print_sentence(&mut s);
    println!("In main: {}", s)
} // x goes out of scope here

fn print_sentence(s: &mut String) {
    s.push_str("Because it's great");
    println!("In function {}", s);
}
```
In code above, string s is declared as a mutable variable and also the function signature is changed to accept a mutable reference to a String. Within the function print_sentence, the value of s is mutated to append a string literal. You will notice that even after the function returns, in the main function, the mutated value of string s is visible and available.

Having mutable references looks convenient, but can be dangerous too. So Rust imposes the following rules so that mutable references cannot be used indiscriminately:  

1. At any given time, you can have either one mutable reference or any number of immutable references.
2. References must always be valid

Let us test the first rule through some code.  

```
fn main() {
    // String type
    let s = String::from("Why is Rust so popular?");

    let x = &s;
    let y = &s;

    println!("x is {} and y is {}", x, y);
} 

```
In code above, we create two variables x and y both of which take immutable references to the same string **s**. The print statement will print the same string for both **x** and **y**.

What we have proven through this code is that it is accceptable to have more than one immutable reference to the same value.

In following code, we declare the variable s as mutable. x is declared as immutable reference to s , while y is declared as a mutable reference to s. What happens when you execute this code?

```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");

    let x = &s;
    let y = &mut s;

    println!("x is {} and y is {}", x, y);
} // x and y go out of scope here
```

You will notice that the compiler throws up an error. This is because Rust does not allow both a mutable reference and immutable reference to the same value in the same scope. It does not make sense to declare x as immutable reference and then have the underlying value of string change through a mutable reference y. 

If we were to extend this example and declare both x and y are mutable variables, again Rust compiler would complain. This is because such code can lead to data race situation, where more than one variable may be  trying to alter the same variable at the same time. This problem can become conspicuous in a multi-threaded scenario where a data element is accessed for mutation by multiple threads without any synchronisation mechanism. 

So, there would be no memory safety if this were to be allowed.

Hence the first rule for using references is:  

1. In any given scope, you can have either one mutable reference or any number of immutable references.

One thing to note is how Rust interprets **scope**. It is intuitive to think of variables enclosed within the same pair of curly braces as part of same scope. But Rust applies another rule also. For Rust, a reference’s scope starts from where it is introduced and continues through the last time that reference is used. So, if we modify example above to split the println statements for x and y as below, you will find that Rust compiler will not complain. This is because, once the first println! macro is executed, the value of x goes out of scope (as it is not used again in the program), so the second mutable reference statement would be accepted as this is the only reference remaining in the program scope.  

```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");

    let x = &s;
    println!("x is {} ", x); // x goes out of scope here

    let y = &mut s;

    println!(" y is {}", y);
} // y goes out of scope here
```

We have seen that Rust will not even compile code with data race conditions! Compare this with other languages where the developer has to debug issues due to data race conditions.

### Slice data type

We have seen how passing a reference to a heap-allocated variable is a way to avoid transferring ownership of that value. We also saw that there are two types of references that can be used - mutable reference and immutable reference.  

We will now look at yet another way to use variables without taking ownership. Rust provides a special data type that is designed to resist ownership. It is the **Slice data type**. A slice , as the word implies, is a portion of data. A string slice is a reference to a part of a string (which is ). An integer slice is a view into an underlying array of integers.  

Let us understand this through a code example.


```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");

    let slice1 = &s[0..3];
    let slice2 = &s[4..];
    let slice3 = &s[..];

    println!("slice 1 is {} ", slice1);
    println!("slice 2 is {} ", slice2);
    println!("slice 3 is {} ", slice3);

    println!("original slice is {} ", s);
} 
```
We declared three different slices into the string **s**. **Slice1** is a view into the first 4 characters of string **s**, **slice2** is a view into all characters of string **s**  starting from the fifth character (not arrays are zero-indexed), and **slice 3** simply says create a view of all characters in string **s**.

When a slice is created, is it like an immutable reference to the string, or is a new copy of the data created? Let us find out. 

```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");

    let y = &mut s;

    println!(" y is {:p}", y.as_ptr());

    let slice1 = &s[0..3];
    let slice2 = &s[5..];
    let slice3 = &s[..8];

    println!("original string is {:p} ", s.as_ptr());
    println!("slice 1 is {:p} ", slice1.as_ptr());
    println!("slice 2 is {:p} ", slice2.as_ptr());
    println!("slice 3 is {:p} ", slice3.as_ptr());
} 

```
```
This is the output of the function:(you may see different memory addresses):
y is 0x55f4a61cda40
original string is 0x55f4a61cda40 
slice 1 is 0x55f4a61cda40 
slice 2 is 0x55f4a61cda45 
slice 3 is 0x55f4a61cda40
```
In code above, we declared a mutable reference to the string s, and also declare three slices over the same string s. If slices were treated as immutable references,then the code below should be disallowed by compiler because one cannot have both a mutable reference and immutable reference to same string in same scope. But surprisingly the compiler is happy with this code. Does this mean that slices are NOT treated like immutable references by Rust? Let us try to find out, by just add one more line to the previous example to print value of y at the end of the program. You will see that the compiler now throws an error. The updated code and error messages are shown below:

```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");

    let y = &mut s;

    println!(" y is {:p}", y.as_ptr());

    let slice1 = &s[0..3];
    let slice2 = &s[5..];
    let slice3 = &s[..8];

    println!("original string is {:p} ", s.as_ptr());
    println!("slice 1 is {:p} ", slice1.as_ptr());
    println!("slice 2 is {:p} ", slice2.as_ptr());
    println!("slice 3 is {:p} ", slice3.as_ptr());
    
    println!("{}", y); // New line added
} 
```

```
Compiling playground v0.0.1 (/playground)
error[E0502]: cannot borrow `s` as immutable because it is also borrowed as mutable
  --> src/main.rs:9:19
   |
5  |     let y = &mut s;
   |             ------ mutable borrow occurs here
...
9  |     let slice1 = &s[0..3];
   |                   ^ immutable borrow occurs here
...
18 |     println!("{}", y);
   |                    - mutable borrow later used here

error[E0502]: cannot borrow `s` as immutable because it is also borrowed as mutable

```
We can see that the compiler confirms that when a slice is created, an immutable borrow occurs. So, what is the reason that code in ownership15.rs works, but ownership16.rs does not? What does the addition of the innocuous println! statement really do ? The answer is contained in a concept called non-lexical lifetime. As per normal rules, in snippet ownership15.rs, the variable y should go out of scope only at the end of the main function. This is called lexical scope. But in Rust 2018 edition, Rust has introduced the feature of non-lexical lifetime(scope), due to which the variable y in snippet ownership15.rs goes out of scope even before the slices are declared, as y is not used elsewhere in the program subsequently. But in snippet ownership16.rs, when we add a statement at the end of the program to print the value of y, then the non-lexical lifetime rule does not apply anymore to variable y, and the compiler complains because in this case, the variable y is still in scope of the program when the slices 1–3 are declared.

### Mutable variable vs Mutable reference
There is another aspect of references that I’d like to highlight. In Rust there is a difference between mutability property of a variable and the mutability property of a reference to the variable. To illustrate this let us make a small change to ownership16.rs (which does not compile, if you recall), to change y from a mutable reference of s to immutable reference of s. You will find that the program now works! This is because even though s is declared as a mutable variable, since y is declared as immutable reference to s, the compiler finds this acceptable as now there are 4 immutable references to s (variables y , slice1, slice2 and slice3), which is as per the ‘law of the land’ in Rust, as there can be any number of immutable references to a variable at the same time in same scope. The code snippet is given below:  

```
fn main() {
    // String type
    let mut s = String::from("Why is Rust so popular?");

    let y = &s; // Change y from mutable pointer to immutable pointer

    println!(" y is {:p}", y.as_ptr());

    let slice1 = &s[0..3];
    let slice2 = &s[5..];
    let slice3 = &s[..8];

    println!("original string is {:p} ", s.as_ptr());
    println!("slice 1 is {:p} ", slice1.as_ptr());
    println!("slice 2 is {:p} ", slice2.as_ptr());
    println!("slice 3 is {:p} ", slice3.as_ptr());
    
    println!("{}", y);
} 
```

### How are slices stored?

Let us now look at how the slices are stored. Is a new value created in a different heap memory area for each slice, or do all the slices point to the original string s?
In snippet ownership15.rs , the variables s and slices 1–3 are printed as pointers . You can observe from program output (ownership 15b.txt) that slices 1 , 2 and 3 are pointing to the same memory address area as the original string s. (note: slice2 has a different memory address to adjust for the offset of 5 bytes, as we created slice2 by reading from byte 5 of string s, but it is still referring to the same area of memory corresponding to string s). This shows that creating a slice does not duplicate the value in another area of heap.  

### Summary of memory safety in Rust

We have seen how through features such as Ownership, borrowing, references and slices,  Rust provides memory safety at compile time. We saw how Rust avoids *double free* memory errors through Ownership rules, and *data race* errors through borrowing & reference rules. 

To take a simple (crude) analogy of using a meeting room, let us say you walk into a meeting room and use the projector, lights and whiteboard. After the meeting when you leave the room , let's see what happens in the context of different programming languages:  

1. In C/C++, you have to manually switch off the projector, turn off the lights and clean the whiteboard.
2. In high-level languages such as Java/C#,Ruby,Python etc, we use a cleaning service (called garbage collector) to clean up after you leave.
3. In Rust, you neither have to clean up manually, nor employ a cleaning service. Instead, there are sensors that detect when you leave the room, the projector switches off , the lights turn off and there is a self-cleaning white board with motors, cleaning rod, cleaning head and control panel. All this is possible using the fantastic Rust compiler!  

Memory management in Rust is roughly analogous to this.  


Let us now see how Rust prevents another class of memory errors - dangling references.  Rust achieves this through the concept of **lifetimes**. We will see this in a future lesson. 




