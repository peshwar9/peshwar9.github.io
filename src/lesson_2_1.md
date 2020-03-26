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
  fn main() {
      // Declare a variable x of type unsigned integer of length 8 bits , and initialize it with value -5.
      let x: i8 = -5;
 
      // Similarly i16, 132, i64 and i128 represent data types of length 16-bits, 32-bits, 64-bits and 128-bits respectively
 
      // Declare a variable x of type signed integer of length 8 bits , and initialize it with value 5
      let y: u8 = 5;
 
     // Similarly u16, u32, u64 and u128 represent data types of length 16-bits, 32-bits, 64-bits and 128-bits respectively

     // Declare a variable of single-precision floating point data type and initialize it with value of 5.0
     let z: f32 = 5.0;
     // Similarly f64 represents double precision floating point data type
     println!("{},{},{:.1}", x, y, z);

     // Declare a variable of type Boolean
     let am_i_successful: bool = true;

     // Declare a variable of type character and initialize it with a letter
     let this_is_a_char: char = 'r';

     println!("{}.{}", am_i_successful, this_is_a_char);

     // Declare a variable of type tuple and initialize it.
     let this_is_a_tuple: (u8, bool, char) = (2, false, 'c');
     println!("The value of tuple is {:?}", this_is_a_tuple);

     // Declare a variable of type arrray and initialize it
     let inner_planets: [&str; 4] = ["Mercury", "Venus", "Earth", "Mars"];
     println!("Inner planets are {:?}", inner_planets);
     //Access the second element of the inner_planets array
     println!("2nd inner planet is {:?}", inner_planets[1]);
 }

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
 use rand::Rng;
 use std::env;
 fn main() {
     // Generate a random number between 1 and 100
     let random_number = generate_random_number();
 
     println!("Random num is {} ", random_number);
 
     let user_input = get_user_input();
  
      println!("User input is {}", user_input);
  
      if (user_input == random_number) {
          println!("Congratulations, you will get 10 times your money");
      } else {
          println!("Sorry, you lose. I will now take your money")
      }
  }
  
  fn generate_random_number() -> u8 {
      let mut rng = rand::thread_rng();
      rng.gen_range(1, 6)
  }
  
  fn get_user_input() -> u8 {
      let user_input = env::args().nth(1).unwrap();
      let to_number: u8 = user_input.parse().expect("Not a number");
      to_number
  }
```

First two lines of code show the imports we are making to bring in Rand crate module and a standard library module into the scope of this source code.  

We then have the main () function, and two other functions for generating random number and to get user input from command line, respectively.  

In the ```main( )``` function, we see code to call the two functions to get random number generated and to get user inputs. This is followed by  the control flow code to check if the user input matches the random number generated, and print out appropriate messages.  

It is a simple program, but serves to demonstrate the concepts discussed so far.


### Tasks for you

1. Extend the betting program to do the following:  
  * Accept 2 pieces of user input - number guessed and bet amount placed. eg user can bet on number 6 with $100.  
  * Generate the random number after accepting the user inputs  
  * If the number guessed is correct, return to user an amount equal to double of the bet placed by user. If the number guessed is within 2 numbers of random number generated, then print message stating that 90% of bet placed will be returned. Otherwise, print a message stating user lost the bet.
2. Extend program above to take 3 inputs from user and check if any of the numbers match the random number generated (Hint: use arrays). Display messages to user accordingly.  


