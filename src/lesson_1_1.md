# Say hello!


### What will you learn?

1. Structure of a minimal rust program
2. What is a  statement in Rust?
3. How to print out a value to the terminal
4. Differences between macros and functions

### Activity

* Write your first program in Rust to print Hello World

### Anatomy of code

```
fn main() {
  println!("Hello world, I have arrived");
}
```

The `fn` keyword indicates it is a function.
`main( )` keyword specifies that code execution will begin here. `main( )` is a function which does not take any parameters, and has no return value.
The pair of curly braces `{ }` encloses the scope of the main function within which the rest of the code will reside.

Now, this empty function really does nothing, because we have not specified any instructions inside the main function. Let us introduce a statement for the program to greet us on start-up, within the main function. In Rust, a statement is an instruction that performs some action and does not return a value. 

`println!("Hello world")`

The statement above contains a built-in macro called `println!` which takes a string value as an argument and prints the string to the terminal. A macro is just a short-hand notation representing a block of code, it is similar to a function but there are a few key differences. Notice that macros in Rust end with a semicolon.
Run the program now. You should see `Hello World` printed in the terminal. 


### Rust Concepts

In Rust, both macros and functions denote blocks of code that perform specific computations and can be called from another section of code. But there are key differences:
Macro is a way of writing code, which in turn generates other rust code (also known as metaprogramming) at compile-time, whereas a function is hand-coded by the developer. At the time of compilation, the compiler replaces the macro name with the actual code used to accomplish that functionality. 
The benefit for the developer is that macros make code shorter and easier to read. The drawback with macros is that they are more complex to conceive and write than functions.

### Tasks for you

1. Print Hello world in any other language.   
*Note that Rust supports both single-byte languages (like English, French, German, Spanish) where each alphabet of the language is stored in a single-byte in memory and double-byte languages (eg, Chinese/Mandarin, Japanese) where each alphabet uses two bytes in memory. This is possible because the char type in Rust is 4 bytes in length.*

2. Print Hello world with emotion by adding emojis 😀.   
*Have you wondered how this actually works? Rust supports unicode in its core character data type, and unicode assigns a unique number for every character, no matter the platform or programming language.*
