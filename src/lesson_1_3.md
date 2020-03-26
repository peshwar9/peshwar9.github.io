# Loops: Say hello continually
### What will you learn?

Three types of loops for repeated operations and calculations- loops, while and for



### Activity

* Write a program to print Hello World multiple times.

### Anatomy of code

To execute a block of code multiple times, Rust has the control flow feature called loops. There are three types of loops in Rust: **loop**, **while** and **for**.  



We will first look at the **loop**.  
The loop keyword denotes an  infinite, unconditional loop .   

```
fn main() {
   loop {
       println!("Hello world!");
   }
}
```

If you execute this program, you will see ```Hello world``` printed repeatedly in your terminal, until you stop the program with **ctrl+c**.  Such a program seems pretty pointless at first glance, but this is more or less how long-running services operate, e.g., web application,   api , smtp servers which keep listening for incoming requests in a continuous loop.  

You can use the **break** statement to exit the loop, and continue to move to the next iteration in loop as shown in example below.  

```
fn main() {
 let mut number_of_times = 0; 
   // Infinite loop
   loop {
       number_of_times += 1;
       if number_of_times == 1 {
           println!("Hello {} time", number_of_times);
           // Skip the rest of this iteration
           continue;
       }
       if number_of_times == 3 {
           println!("Goodbye {} times", number_of_times);
           // Exit this loop
           break;
       }
   }
}
```

Let us next look at conditional loops with the **while** keyword. It is used to check for a condition inside the loop.  

```
fn main() {
   let mut number_of_times = 5;
   while number_of_times > 0 {
       println!("Hello World");
       number_of_times -= 1;
   }
   println!("Good bye!!!");
}
```

If you run the program, you will see ```Hello World``` printed five times in the terminal, followed by ```Good bye```.  

The last type of looping construct we will see is the **for** loop.

```
fn main() {
   let greetings = ["Hello", "Hola", "Bonjour", "Hallo", "こんにちは"];
   for greeting in greetings.iter() {
       println!("{}", greeting);
   }
}
```

```For.. in``` loop is a very safe and concise way to iterate through collections. The **iter()** method is available over collection data types , such as the array and this makes it very pleasant to write and read code compared to equivalent **for loop** constructs in many other programming languages.   
If you run the program, you will see each element of the ```greetings``` array printed in a sequence as the ```for``` loop executes.  
Another way to use the ```for``` loop is to use a range notation ```x..y```, as follows:  

```
for n in 1..5 {
 println!("Hello world {}",n)
}
```



### Tasks for you

1. Declare an array with a list of names. Use a suitable looping construct to iterate through the array and print out a greeting for each name.
2. Write a program to iterate through numbers 1 to 100 and print out  the square of the number  if the number is even, and the cube of the number if the number is odd.
