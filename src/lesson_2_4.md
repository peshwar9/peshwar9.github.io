# Data types: structs, enums and traits

### What will you learn?

1. Structs, enums and traits in Rust

### Activity

* Write programs to understand how to define aggregate custom data types in Rust and to specify state and behaviour.

### Introduction

Every programming language provides features that enable representation of digital models of real-world objects. Object oriented programming languages achieve this by viewing the world as a set of interacting objects, each with its own behaviour (methods), data (properties) and relationships (inheritance and composition).  

Rust is not an object-oriented programming language, but provides the ability to specify state, behaviour and relationships of objects (run-time entities)in a different way. After all, how can a programming language be useful if it cannot allow us to model real-world objects and their behaviour? If I cannot represent a bank, an airline, a hotel, a car, a user, a tweet, a post , a message, a product or a transaction in a programming language easily, then I cannot do many useful things with it.  

In order to represent a real-world object, we cannot work with primitive data types such as integer, character or boolean. We need aggregate data types that can contain various properties of objects. Take an example of a car. A car could have multiple properties such as registration number (string data type), make & model (strings again) , engine horsepower (integer), fuel type — petrol/diesel (enumeration data type), colour (hex color code), number of passengers (integer), price (float), status (sold/readyForSale) etc. A car can thus be represented as an aggregate data type containing various properties, each of which is either a primitive data type or some other custom-defined data type.  

### Struct
Rust allows us to represent an object such as a car in the form of a struct data type , as shown below.

```
struct Car {
 registration_number : String,
 make: String,
 model: String,
 horse_power: u8,
 fuel_type: String,
 number_of_passengers: u8,
 price: f32,
 status: String
}
```
To use a struct after it is defined, it has to be instantiated by specifying values for each of the fields in the struct.

```
fn main() {
    let _car1 = Car {
        registration_number: String::from("AST 1234"),
        make: String::from("Ferrari"),
        model: String::from("F8 Tributo"),
        horse_power: 710,
        fuel_type: String::from("Petrol"),
        number_of_passengers: 2,
        price: 293480.0,
        status: "For Sale"
    };
}
```

The set of values associated with the various fields of **car1** object represents the state of **car1** at any point in time.  
Are structs just static objects, or can they have behaviour associated with them? There are two classes of behaviour a struct can have in Rust — methods and associated functions.  

Here is an example of a method:

```
impl Car {
 fn calculate_price(&self, quantity: u32) -> f32 {
 let quantity = quantity as f32;
 self.price * quantity
 }
}
```
Methods are declared with the **fn** keyword and their name. They can have parameters and return value and perform some computation. The first parameter of a method is always **self**, which represents the instance of the struct the method is called on. Struct methods are enclosed within an impl block. (impl stands for implementation).  

Here is how to invoke the method on struct car:

```
car1.calculate_price(5) // To calculate price of 5 cars
```

We can also define an associated function for a struct. Associated function is different from a method in that an associated function is like a static method on an object (for those familiar with OO languages). Associated functions operate on the struct definition while methods operate on a struct instance. Here is an example of an associated function called generate_warranty_statement() for a car, invoked using the double-colon notation on the struct Car.  

```
Car::generate_warranty_statement()
```

While the fields defined as part of a struct definition denote the definition of state that is associated with it, the methods and associated functions defined as part of the impl block of the struct denote the behaviour associated with the struct.  

What we have seen so far is a classic struct. There are two other types of structs:  

1. Tuple struct
2. Unit structs

A car object defined as a tuple struct in rust would look as follows:

```
struct Car(String, String, String, u16, String, u8, f32);
```

Shown below is how to instantiate a tuple struct. Note the main difference between regular and tuple structs is that in the latter , the fields are not named.  

```
let car2 = Car(
 String::from(“AST 1234”),
 String::from(“Ferrari”),
 String::from(“812 GTS”),
 812,
 String::from(“Petrol”),
 2,
 293480.0,
 );
```

The third type of struct is called Unit struct, and is defined as follows:  
```
struct Car();
```

The unit struct is instantiated as follows:  

```
let car3 = Car();
```

Unit structs are used in places where there is no field/data to be stored , but useful to specify behaviour of generics, which will be covered in a future post.  

Here is the full code

```
// Struct definition showing the fields (state)

struct Car {
    registration_number: String,
    make: String,
    model: String,
    horse_power: u16,
    fuel_type: String,
    number_of_passengers: u8,
    price: f32,
}

// Struct methods and associated functions showing behaviour of the struct

impl Car {
    
    // This is a method
    
    fn calculate_price(&self, quantity: u32) -> f32 {
        let quantity = quantity as f32;
        self.price * quantity
    }
    
    // This is an associated function

    fn generate_warranty_statement() -> &'static str {
        "This car carries a warranty of 5 years against any manufacturing defects"
    }
}

// Define a Tuple struct

struct TCar(String, String, String, u16, String, u8, f32);

// Define a unit struct

struct UCar();

fn main() {
    
    // Instantiate the car struct
    
    let car1 = Car {
        registration_number: String::from("AST 1234"),
        make: String::from("Ferrari"),
        model: String::from("F8 Tributo"),
        horse_power: 710,
        fuel_type: String::from("Petrol"),
        number_of_passengers: 2,
        price: 293480.0,
    };
    
    // Instantiate the car tuple struct
    
        let _car2 = TCar(
        String::from("AST 1234"),
        String::from("Ferrari"),
        String::from("812 GTS"),
        812,
        String::from("Petrol"),
        2,
        293480.0,
    );

    // Instantiate car unit struct
    
    let _car3 = UCar();
    
    // Calculate price of 5 Ferrari f8 Tributos.
    println!("Price of 5 ferraris is : $ {}", car1.calculate_price(5));
    
    // Generate the warranty statement for all cars sold from this dealer

    println!(
        "Warranty statement for Ferrari F8 Tributo is : {}",
        Car::generate_warranty_statement()
    );
}
```

### Enum

Enums or enumerations are custom data types in Rust (like in other mainstream programming languages). the set of values that an enum can take is restricted and predefined. Each possible value an enum can take is called a variant.  

Let us look at how we can use enums. In the example above, in the car struct, we have a field called **fuel_type**. Let’s take the case of the car manufacturer making cars of the following four types: Petrol, Diesel, Electric and Hybrid. Each of these types is called an invariant. Here’s how we would represent it programmatically:  

```
enum FuelType {
 Petrol,
 Diesel,
 Hybrid,
 Electric,
}
```

Basically we are declaring that there are only four possible fuel types that canbe associated with this enum called FuelType. Next, we need to redefine the Car struct to take a field type of FuelType for field with name fuel_type (instead of String).  

```
struct Car {
 registration_number: String,
 make: String,
 model: String,
 horse_power: u16,
 fuel_type: FuelType, // this was earlier String type 
 number_of_passengers: u8,
 price: f32,
}
```

This is how we would instantiate the car now:  

```
let car1 = Car {
 registration_number: String::from(“AST 1234”),
 make: String::from(“Ferrari”),
 model: String::from(“F8 Tributo”),
 horse_power: 710,
 fuel_type: FuelType::Petrol, // Note how enum value is assigned 
 number_of_passengers: 2,
 price: 293480.0,
 };
```

If we try to assign a value to fuel_type that is not one of the 4 defined values in enum FuelType definition, our(friendly) compiler would refuse to accept it.  

Using enums improves code safety because we can be sure that no programmer assigns an invalid value to this field resulting in unpredictable behaviour. Why is this so important? Let’s take an example. Car manufacturers have to perform emission tests on the vehicles to meet regulatory requirements. Each fuel type may have different sets of metrics to be reported. And reporting should cover all vehicles of all fuel types.   

Imagine a case where an invalid fuel type is entered by data entry operator for a batch of cars. These cars would then not even appear in the regulatory reporting which could cause serious compliance issues. An enum is one way to avoid getting into that situation. (this by the way is not an exaggerated scenario, data entry errors happen all the time in enterprise systems leading to many kinds of error scenarios not envisaged at the time the system was designed and developed).  

Enums in Rust have an additional property not common in other programming languages. The variants an enum can take , also can accept some associated data. Again, this is best explained with an example.  

In the previous example, for electric vehicles, let’s assume we also want to track what is the type of battery used by the car, and do some processing based on that. A car battery can be Lithium Ion (Li-Ion), Lithium Sulphur (Li-S) or Nickel Metal Hydride (Ni-MH). How do we specify this in the enum definition? Here is how to do it.  

```
enum FuelType {
 Petrol,
 Diesel,
 Hybrid,
 Electric(String),
}
```

This is how you would instantiate a new car now:

```
let car1 = Car {
 registration_number: String::from(“AST 1234”),
 make: String::from(“Ferrari”),
 model: String::from(“F8 Tributo”),
 horse_power: 710,
 engine_type: FuelType::Electric(String::from(“Lithium Ion”)),
 number_of_passengers: 2,
 price: 293480.0,
 };
```

This feature can be quite useful to do some processing based on battery type.  

Here is the complete code using enums:  

```
// Define an enum for engine type

#[derive(Debug)]  // this macro enables printing of the value of enum in debug mode
enum FuelType {
    Petrol,
    Diesel,
    Hybrid,
    Electric(String),
}

#[derive(Debug)]

struct Car {
    registration_number: String,
    make: String,
    model: String,
    horse_power: u16,
    engine_type: FuelType,
    number_of_passengers: u8,
    price: f32,
}

impl Car {
    fn calculate_price(&self, quantity: u32) -> f32 {
        let quantity = quantity as f32;
        self.price * quantity
    }

    fn generate_warranty_statement() -> &'static str {
        "This car carries a warranty of 5 years against any manufacturing defects"
    }
}

struct TCar(String, String, String, u16, FuelType, u8, f32);

struct UCar();



fn main() {
    let car1 = Car {
        registration_number: String::from("AST 1234"),
        make: String::from("Ferrari"),
        model: String::from("F8 Tributo"),
        horse_power: 710,
        engine_type: FuelType::Electric(String::from("Lithium Ion")),
        number_of_passengers: 2,
        price: 293480.0,
    };

    let _car2 = TCar(
        String::from("AST 1234"),
        String::from("Ferrari"),
        String::from("812 GTS"),
        812,
        FuelType::Petrol,
        2,
        293480.0,
    );

    let _car3 = UCar();
    println!("Car 1 is {:?}", car1);
    println!("Price of 5 ferraris is : $ {}", car1.calculate_price(5));
    println!(
        "Warranty statement for Ferrari F8 Tributo is : {}",
        Car::generate_warranty_statement()
    );
}
```

### Traits

A trait defines a set of related functionality for a data type. The functionality is described as a set of methods. Traits can be implemented for any data type. Traits are a great way to describe common behaviour shared by many user-defined types (eg structs). This is like polymorphism in object oriented programming, or interfaces in a few other languages (like Go). Let us see an example of how to enhance behaviour of car struct using traits. Let’s consider a (hypothetical) requirement that in a particular country , a vehicle manufacturer has to report on Co2 emissions and Fuel efficiency checks done on every single vehicle being rolled out on the road. How would we implement it?  

The vehicle manufacturer wants to comply with the regulatory requirements for all classes of vehicles manufactured — cars, buses and trucks. A trait is a good way to define the common set of regulatory requirements across vehicle categories:  

```
trait EnvReg {
 fn fuel_efficiency_check(&self, fuel_consumption: f32) -> bool;
 fn co2_emission_check(&self, co2_emission: u8) -> bool;
}
```

We have defined a trait above called **EnvReg** (short for Environmental regulations) which has two methods. First is *fuel efficiency check* and the second is *Co2 emission check*. Note there is no implementation of these methods specified in trait definition, and the method signatures end with a semicolon.  

Now let us implement this functionality for cars. The syntax is **impl <trait-name> for Car {}**   

```
impl EnvReg for Car {
    fn fuel_efficiency_check(&self, fuel_consumption: f32) -> bool {
        // For petrol vehicles , fuel consumption should be 4.1 liters for 100 kms or less
        if self.fuel_type == FuelType::Petrol {
            if fuel_consumption <= 4.1 {
                return true;
            } else {
                return false
            }
            // For diesel vehicles , fuel consumption should be 3.6 liters for 100 kms or less
        } else if self.fuel_type == FuelType::Diesel {
             if fuel_consumption < 3.6 {
                return true;
            } else {
                return false
            }
        }
        return false;
    }

    fn co2_emission_check(&self, co2_emission: u8) -> bool {
    // For all cars, carbon emission should be less than or equal to 95g per km.
        if co2_emission <= 95 {
            return true;
        }
        return false;
    }
}
```

The code is self-explanatory and annotated. Basically given a value from field test, the methods return whether a given vehicle conforms to fuel-efficiency and co2 emission norms based on a set of rules.  

The methods on traits can be invoked just like regular methods on a trait.  

```
println!(
 “Fuel efficiency test succeeded? {} “,
 car1.fuel_efficiency_check(4.3)
 );
 //print result of co2 emission test for car
 println!(
 “Carbon emission test succeeded? {} “,
 car1.co2_emission_check(92)
 );
```

The vehicle manufacturer can now implement the same trait on the **bus struct** and **truck struct** too. The advantage of traits is that a common set of behaviours can be centrally defined and each type can implement these behaviours in a way that makes sense for that type. For example, the implementation of fuel efficiency checks for trucks could be different because it would be a heavy-duty vehicle where different metrics may apply. But by encoding a common set of environmental regulatory requirements as traits, the vehicle manufacturer can mandate that all vehicle types must implement all functionality associated with that trait, thus avoiding compliance risk.  

Here is the complete code:  

```
#[derive(Debug, PartialEq)]
enum FuelType {
    Petrol,
    Diesel,
    Hybrid,
    Electric(String),
}

// Define a trait

trait EnvReg {
    fn fuel_efficiency_check(&self, fuel_consumption: f32) -> bool;
    fn co2_emission_check(&self, co2_emission: u8) -> bool;
}

#[derive(Debug)]

struct Car {
    registration_number: String,
    make: String,
    model: String,
    horse_power: u16,
    fuel_type: FuelType,
    number_of_passengers: u8,
    price: f32,
}

impl Car {
    fn calculate_price(&self, quantity: u32) -> f32 {
        let quantity = quantity as f32;
        self.price * quantity
    }

    fn generate_warranty_statement() -> &'static str {
        "This car carries a warranty of 5 years against any manufacturing defects"
    }
}

impl EnvReg for Car {
    fn fuel_efficiency_check(&self, fuel_consumption: f32) -> bool {
        // For petrol vehicles , fuel consumption should be 4.1 liters for 100 kms or less
        if self.fuel_type == FuelType::Petrol {
            if fuel_consumption <= 4.1 {
                return true;
            } else {
                return false;
            }
        // For diesel vehicles , fuel consumption should be 3.6 liters for 100 kms or less
        } else if self.fuel_type == FuelType::Diesel {
            if fuel_consumption < 3.6 {
                return true;
            } else {
                return false;
            }
        }
        return false;
    }

    fn co2_emission_check(&self, co2_emission: u8) -> bool {
        // For all cars, carbon emission should be less than or equal to 95g per km.
        if co2_emission <= 95 {
            return true;
        }
        return false;
    }
}

fn main() {
    let car1 = Car {
        registration_number: String::from("AST 1234"),
        make: String::from("Ferrari"),
        model: String::from("F8 Tributo"),
        horse_power: 710,
        fuel_type: FuelType::Electric(String::from("Lithium Ion")),
        number_of_passengers: 2,
        price: 293480.0,
    };

    // Print the values of car 1
    println!("Car 1 is {:?}", car1);

    //print the price of 5 Farraris
    println!("Price of 5 ferraris is : $ {}", car1.calculate_price(5));

    //print the warranty statement for Cars
    println!(
        "Warranty statement for Ferrari F8 Tributo is : {}",
        Car::generate_warranty_statement()
    );

    // Print result of fuel efficiency for car
    println!(
        "Fuel efficiency test succeeded? {} ",
        car1.fuel_efficiency_check(4.3)
    );

    //print result of co2 emission test for car
    println!(
        "FCarbon emission test succeeded? {} ",
        car1.co2_emission_check(92)
    );
}
```

The code listing contains macros derive(Debug) and derive(PartialEq) which are used here for printing values to standard output and for checking equality conditions for the enum values respectively. You can ignore them for purposes of understanding the topic of this post.  

### Conclusion

Using the example of a vehicle manufacturer, we have seen how in Rust ,
* **structs** can be used to model cars and define *state*
* methods and associated functions can be used to specify their *behaviour*
* **enums** can be used to specify range of allowed values for a custom data type
* **traits** can be used to describe shared behaviours across user-defined data types


These are powerful features that enable modelling of even complex problem domains without a lot of boilerplate code.  
