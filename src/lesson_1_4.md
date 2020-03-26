# Functions & closures: Say hello smartly
### What will you learn?

1. Functions
2. Closures


### Activity

* Write a program to print Hello World using functions and closures

### Anatomy of code

Functions are blocks of code that are identified by a name. We have seen earlier that every Rust program starts with the ```main()``` function. Here is a simple example of how to define and invoke a function in Rust.   

```
fn main() {
 say_hello();
}
fn say_hello() {
println!("Hello World - I'm functional");
}
```

Function declarations start with the ```fn``` keyword, followed by a set of **parentheses** to enclose optional parameters passed to the function, and then a set of **curly braces** to enclose the actual code.  Parameters indicate the variables that are declared as part of a function’s signature.  

 To invoke the function, use the function name followed by a set of parentheses with optional arguments. Arguments represent the concrete values that are passed to the function at time of invocation. The example below demonstrates how to pass parameters to a function. Note that in function declaration, the data type of parameters is mandatory. This is a safety feature because the compiler can then check for discrepancies in usage and provide warning.  


```
fn main() {
 let language = "German";
 say_hello(language);
}
fn say_hello(lang: &str) {
println!("Hello World - I'm learning {}", lang);
}
```

Note that function names use snake case. Functions can be declared anywhere in the source code.  
Within the body of the function, statements and expressions can be specified. Recall that a statement is an instruction to the computer to perform some actions and statements do not return a value. In the previous example, printing out a value within the ```say_hello``` function code is an example of a statement, as it does not return any value. Note that statements end with a semicolon.   
On the other hand, expressions are computations that evaluate to a value which can be returned.   
Let us alter the previous code to have the ```say_hello``` function return a value, instead of printing it out.  

```
fn main() {
 let language = "German";
 println!("{}",say_hello(language));
}
fn say_hello(lang: &str) -> String {
format!("{} {}", "Hello I'm learning" , lang)
}
```

In the example above, note the following:  
We have added a return value from the ```say_hello``` function using the ```-> String``` annotation to denote that the return value from the function is a value of type ```String```.   
We are using the ```format!``` macro to concatenate a fixed string with the value passed to the function for ```lang``` parameter. This is an example of an expression within function code because it evaluates to a value. The value so evaluated is returned from the function. Note the lack of  explicit return statement specified within the function, and also the absence of a semicolon at the end of the ```format!``` indicates to Rust that this is an expression and the value computed by the expression is automatically returned from the function.  
Let us now take a look at another concept in Rust , **closures**.
**Closures** are similar to **functions**, with a few key differences. Let us look at some code  first.  


```
fn main() {
  // Closure declaration
 let print_hello = |lang: &str| {
   println!("{}", format!("{} {}", "Hello I'm learning" , lang))};
// Call the closure with language as argument
 print_hello("German");
 print_hello("French")
}
```

We are declaring a **closure** which is **anonymous** , i.e., it does not have an identifier, unlike a function. The closure accepts a parameter ```lang``` of string literal type, concatenates a fixed string to the ```lang``` parameter (using the ```format!``` macro), and then prints it out to the terminal (with the ```println!``` macro).  
The **closure** itself is stored in a variable called ```print_hello``` and is not executed at time of declaration of the ```print_hello``` variable. Instead,  the **closure** is executed only when we call the closure using ```print_hello(“German”)``` or ```print_hello(“French”)```.  
Interestingly in **closures**, we don’t even have to explicitly pass the input parameter, because **closures** have the ability to ‘capture’ the outer environment variables. So the following code also would work.  

```
fn main() {
  let lang = "German";
 // Closure declaration
 let print_hello = ||
   println!("{}", format!("{} {}", "Hello I'm learning" , lang));
// Call the closure with language as argument
 print_hello();
}
```

Note that we are not passing the ```lang``` argument when we invoke the ```print_hello()``` **closure**. Also in the definition of **closure**, we are not specifying any input parameters (denoted by | | ), but the **closure** definition is able to ‘capture the lang parameter from its surrounding environment’.  
We can summarize the key difference between **functions** and **closures** based on the examples above:  
1. Closures are functions that can capture values from the surrounding (or enclosing) environment.
2. Closures are anonymous (i.e. they are not named like functions)
3. Closures also take input parameters (optionally) .
4. Closure return values can be inferred automatically.
5. Usage of | | instead of ( ) to enclose input arguments
6. Function body required to be enclosed in { } but this is optional for closures when there is only a single expression.


### Tasks for you

1. Write a **function** called ```get_lang_string``` that takes a ```language``` parameter and returns a string containing a greeting in that language.  Invoke this function from ```main()``` and print out the value received from function.
2. Write the same function as a **closure** and invoke it.

