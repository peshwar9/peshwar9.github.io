# Control Flow : Say hello conditionally
### What will you learn?

1. Declaring a variable and assigning it a value
2. Executing code by evaluating a condition
3. Difference between mutable and immutable variables, and why immutability of variables is important for program safety


### Activity

* Write a program to print Hello World in different languages based on evaluating a condition.

### Anatomy of code

Control flow is the feature of a programming language that let’s the developer decide whether to execute some code only once, repeatedly or only under certain conditions. In Rust, control flows are enabled by **if expressions** and **loops**.  
Let’s first declare a variable.  
Variables are used to store values inside a rust program. The following is an example of a let statement that creates a variable called ```am_i_happy``` and initializes it with the value ```Yes```.  

```
let am_i_happy = "Yes";
```

By default , variables are **immutable** in rust. What this means is that the following line of code will show a compilation error, because ```am_i_happy``` is immutable (by default).  

```
let am_i_happy = "Yes";
am_i_happy = "No";
```

Compiler error shown below

 ```
 --> main.rs:4:5
  |
3 |     let am_i_happy = "Yes";
  |         ----------
  |         |
  |         first assignment to `am_i_happy`
  |         help: make this binding mutable: `mut am_i_happy`
4 |     am_i_happy = "No";
  |     ^^^^^^^^^^^^^^^^^ cannot assign twice to immutablevariable

error: aborting due to previous error
```

Use the **mut** keyword explicitly in variable declaration, if you want to modify the value of a variable after initial assignment.

```
let mut am_i_happy = "Yes";
```

Getting back to our original goal of learning how to conditionally print , let us look at the second part of the code    

```
if am_i_happy == "Yes" {
       println!("Hello World! 😀");
   } else {
       println!("Hello World! 😩");
   }
```

If expressions allow the developer to specify that a portion of code should be executed only when a specific condition is met. Note there is no parenthesis for the condition being evaluated in rust in **if expression**, and there is a block of code after each condition.  

You can also specify multiple conditions by using **else if** expressions . When the program is run, it checks each condition in a sequence and executes code block corresponding to the first condition which evaluates to true.  

```
   if am_i_happy == “True” {
       // execute the code in this block
   } else if am_i_happy = “Maybe” {
       // execute the code in this block
   } else if am_i_happy == “False” {
       // execute the code in this block
   }
```

Note that the condition to be evaluated can be either a variable of boolean type, or it can be an expression that evaluates to a boolean. A boolean type takes only one of two values - true & false.  

If the variable ```am_i_happy``` was declared to be of boolean type, the if expression would look like below:  

```
let am_i_happy = true;
```

In the declaration above, ```am_i_happy``` is evaluated to ```boolean``` type through **type inference** by the compiler because the  keyword ```true``` is used to initialize the variable, even though we have not explicitly stated the data type.  
Then the if statement would change to:

```
   if am_i_happy {
       println!("Hello World! 😀");
   } else {
       println!("Hello World! 😩");
   }
```

There is another interesting way to use the if expression.  As an expression in Rust evaluates to a value, the if expression can be used to initialize a value as part of a variable declaration as follows.  

```
   let i_won_competition = false;
   let am_i_happy = if i_won_competition {
     true
   } else {
     false
   };
   println!("Am i happy is {}", am_i_happy);
```

Note that while there is a semicolon after the ```if else``` expression, there is no delimiter after ```true``` and ```false``` keywords.

### Rust Concepts

**Why is immutability a virtue?** The value associated with a variable at a particular point in time represents a particular state of the program at that time. Each time a variable changes its value , the program can go to a different state. This is important to understand because all programs are essentially finite state machines. The behaviour of a program at any point of time is dependent upon any external inputs the program receives and the current state it is in.  
For example , in an order management system, if the order has already been executed (i.e. it is in the executed state), it cannot be cancelled (i.e it cannot move to a cancelled state).  If over course of time, one of the new developers in the project accidentally changes the state of the order to cancelled, the order system can lose its data integrity and cause havoc in the order workflow resulting in customer dissatisfaction. This is the reason why Rust makes all variables immutable by default, and forces a developer to decide upfront whether a variable’s value will change over the scope it is declared in, or not.  
Ofcourse, we cannot have business processes without mutable variables, but Rust requires the developer to explicitly specify the mut keyword on a need basis to enhance program safety.  




### Tasks for you

1. Write a program to declare a variable called ```language``` , give it an initial value, and then print ```Hello world``` in different languages based on the value stored in the ```language``` variable. Write code to print ```Hello World``` in up to 4 different languages using ```if..``` and ```else if..``` expressions.  
2. In the program above, try to alter the value of the ```language``` variable after the initial declaration. Read through the compilation error and see how descriptive and helpful the Rust compiler messages are. Then change the declaration of ```language``` variable to  ```mutable``` , and see the results.