# Data types : Let's play a betting game  

### What will you learn?

1. Data types in Rust - scalar and compound types


### Activity

* Write a program to print Hello World using functions and closures

### Anatomy of code

Let us understand how variables are declared with a specific type.
In Rust, every value which is part of a program is of a particular data type. This is because Rust is a statically typed language and the compiler must be told (or must be able to infer) the data type of every single value while creating a binary or library. Broadly, there are two categories of data types in Rust- **scalar** and **compound**.  

A variable of **scalar data type** holds a single value while a variable of **compound type** holds multiple values.  

There are four **scalar** data types - **integers**, **floating point numbers**, **booleans** and **characters**.   

* **Integers** are non-decimal numbers that can be either positive or negative. 

* **Floats** are decimal numbers.  

* **Boolean** is a type that can have either of only two values - true or false.

* A **character** stores a single alphabet or letter (internally using  its corresponding unicode value).   

There are two **compound** data types - **tuples** and **arrays**.   

* A **tuple** is a collection of values of different types.   

* An **array** is a collection of values of the same type.  

Let us look at examples of declaring each of these data types in code listing below:  


```
 1 fn main() {
 2     // Declare a variable x of type unsigned integer of length 8 bits , and initialize it with value -5.
 3     let x: i8 = -5;
 4
 5     // Similarly i16, 132, i64 and i128 represent data types of length 16-bits, 32-bits, 64-bits and 128-bits respectively
 6
 7     // Declare a variable x of type signed integer of length 8 bits , and initialize it with value 5
 8     let y: u8 = 5;
 9
10     // Similarly u16, u32, u64 and u128 represent data types of length 16-bits, 32-bits, 64-bits and 128-bits respectively
11
12     // Declare a variable of single-precision floating point data type and initialize it with value of 5.0
13     let z: f32 = 5.0;
14     // Similarly f64 represents double precision floating point data type
15     println!("{},{},{:.1}", x, y, z);
16
17     // Declare a variable of type Boolean
18     let am_i_successful: bool = true;
19
20     // Declare a variable of type character and initialize it with a letter
21     let this_is_a_char: char = 'r';
22
23     println!("{}.{}", am_i_successful, this_is_a_char);
24
25     // Declare a variable of type tuple and initialize it.
26     let this_is_a_tuple: (u8, bool, char) = (2, false, 'c');
27     println!("The value of tuple is {:?}", this_is_a_tuple);
28
29     // Declare a variable of type arrray and initialize it
30     let inner_planets: [&str; 4] = ["Mercury", "Venus", "Earth", "Mars"];
31     println!("Inner planets are {:?}", inner_planets);
32     //Access the second element of the inner_planets array
33     println!("2nd inner planet is {:?}", inner_planets[1]);
34 }

```

Let’s first define the type of betting system. This program will implement the following betting logic:  

1. Generate a random number between 1 and 100.  

2. Accept inputs from user whether the number generated is less than or greater than 50.  

3. Compare user input with number generated.  

4. Print a result on terminal to say whether the user has won or lost the bet.

This is a fairly simple program, but we can illustrate several key Rust concepts using this example.  

First we need to figure out how to generate a random number. For this, we can use a built-in function in Rust. Built-in functions can be found in the Rust **standard library**. The functions in standard library are organised in the form:  

**std::module_name::function_name**  

Rust standard Library documentation is available [here](https://doc.rust-lang.org/std/). But there is no suitable function available in the standard library for random number generation. So will use a third party library (called a **crate** in Rust lingo). Let's walk through the code:  



```
  1 use rand::Rng;
  2 use std::env;
  3 fn main() {
  4     // Generate a random number between 1 and 100
  5     let random_number = generate_random_number();
  6 
  7     println!("Random num is {} ", random_number);
  8 
  9     let user_input = get_user_input();
 10 
 11     println!("User input is {}", user_input);
 12 
 13     if (user_input == random_number) {
 14         println!("Congratulations, you will get 10 times your money");
 15     } else {
 16         println!("Sorry, you lose. I will now take your money")
 17     }
 18 }
 19 
 20 fn generate_random_number() -> u8 {
 21     let mut rng = rand::thread_rng();
 22     rng.gen_range(1, 6)
 23 }
 24 
 25 fn get_user_input() -> u8 {
 26     let user_input = env::args().nth(1).unwrap();
 27     let to_number: u8 = user_input.parse().expect("Not a number");
 28     to_number
 29 }
```

Lines 1-2 show the imports we are making to bring in Rand crate module and a standard library module into the scope of this source code.  

Lines 3 - 18 correspond to the ```main( )``` function.  

Lines 20-23 and 25-29 are the two functions for generating random number and to get user input from command line, respectively.  

In the ```main( )``` function, lines 5 and 9 contain code to call the two functions to get random number generated and to get user inputs.  

Lines 13-17 show the control flow code to check if the user input matches the random number generated, and print out appropriate messages.  

It is a simple program, but serves to demonstrate the concepts discussed so far.


### Tasks for you

1. Extend the betting program to do the following:  
  * Accept 2 pieces of user input - number guessed and bet amount placed. eg user can bet on number 6 with $100.  
  * Generate the random number after accepting the user inputs  
  * If the number guessed is correct, return to user an amount equal to double of the bet placed by user. If the number guessed is within 2 numbers of random number generated, then print message stating that 90% of bet placed will be returned. Otherwise, print a message stating user lost the bet.
2. Extend program above to take 3 inputs from user and check if any of the numbers match the random number generated (Hint: use arrays). Display messages to user accordingly.  


