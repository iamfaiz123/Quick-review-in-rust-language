# Rust Syntex
## variables declaration
all variables in rust a declear with the keyword let
`let a = 45;`
`let b = "Apple";`

All tough rust is static typed language. Programmers do not need to specify the type of the variables when declaring them in most cases.
Here is how to attaonate the type of a variabe
`let a:i32 = 10;`
`let b: &str = "Apple";` (quick fact : rust has two types of strings ie String and &str also called as string slice )

## immutability in rust
Rust variables are immutable by default, therefore we must declare whether they can be changed during their declaration.
`let a = 10; a = 12;`

the above program will produce error when compile
cannot assign twice to immutable variable `a`

To solve the aforementioned issue, the user should declare the variable as
`let mut a = 10;`

you can declare a new variable with the same name as a previous variable knwown as shadowing in rust, Shadowing can also have block scope

# data types in rust
Premative datat ypes:

`bool : The boolean type.`
`char : A character type.`
`i8 : The 8-bit signed integer type.`
`i16 : The 16-bit signed integer type.`
`i32 : The 32-bit signed integer type.`
`i64 : The 64-bit signed integer type.`
`isize : The pointer-sized signed integer type.`
`u8 : The 8-bit unsigned integer type.`
`u16 : The 16-bit unsigned integer type.`
`u32 : The 32-bit unsigned integer type.`
`u64 : The 64-bit unsigned integer type.`
`usize : The pointer-sized unsigned integer type.`
`f32 : The 32-bit floating point type.`
`f64 : The 64-bit floating point type.`
`array : A fixed-size array, denoted [T; N], for the element type, T, and the non-negative` `compile-time constant size, N.`
`let a:[i32;2] = [1,2]; declearation of array`
`slice : A dynamically-sized view into a contiguous sequence, [T].`
`str : String slices.`
`tuple : A finite heterogeneous sequence, (T, U, …).`


facts
rust have type conversion using `into()` and `from()` trait, most of the premitive data types have these method implemeted by default
you can do it explicity using as key word let `a = 10 as i32` , under the hood as keyword itself call from method on the data type, these are some examples of Zero cost abstraction in rust
# Custom data types in rust
`Struct`
`Enum`

struct
Structs in rust are more like class in other OOP language, Struct has static function ( know as associated function in rust ) and methods

 `struct MyStruct {
   A:i32,
   B:String,
   C:f64
}`
in the above code we defined a struct with name MyStruct , we can create an instance of the struct as

 ~~~ 
 let a = MyStruct{
      A: 10,
      B : "Apple".to_string(),
      C : 10.into()
  }
  ~~~
the data type of the variable a will be MyStruct
to create methods on Struct we need to `impl` it
~~~
 impl MyStruct {
     fn Associated(parameters)-> < data type >{
         
     }
     fn method(&self)-> < data type >{
         
     }
 }
 ~~~
Associated function as called using `MyStruct::Associated()` while method can be called as `a.method()`
where `a` is instance of struct MyStruct
methods can be declear in many ways
`fn method(&self)` when you one wants object reference to read data
`fn method(self)` when you want to consume the object
`fn method(mut &self)` when you want to mutate the object
read function defination to understand more

# functions in rust
We define a function in Rust by entering fn followed by a function name and a set of parentheses. The curly brackets tell the compiler where the function body begins and ends.
`fn Callme()->i32 { code block }`
in the above code the fn keyword is used to declar function followed by function name and parameters in closed bracket `()` and `->` for function return type , return type can be discared if the function does not return anything
~~~
fn calculate_sum(A:i32,B:i32)->i32 {
    return A+B;
} 
~~~
The above fn takes two argument and calculate the sum of the and returns it, it must the noted that the return type of the function must be declarer

if we remove the ; from an expression rust compiler will consider that expression as return value of the function
ie we can return the value as `return A+B;` or just` A+B`
The parameters in in function declearation it self are immutable by default
~~~
fn callme(A:i32){
A = 11;
} 
~~~
the above code will throw an error since the declear varaiable ( parameters )is not mutable, to fix this the function decleartion should be
`fn callme(mut A: i32)`

quick facts

function are ownable type, ie the function will take the ownership of the variables passed
variables declear in function has function scope
since rust support dynamic dispatch function can also return vartiables that does implement some trait,` -> impl thisTrait`
Just like javascript rust also have async fn which uses .await and return a type knwown as future
more about trait and ownership later
# Comments in rust
Like every other programing language rust support line and multiline comments
`// `fro single line comment
`///` for multi line comment

# control statements in rust
rust have few control statemets
`if`
`if let`
`loop`
`while`
`match`
are one of most used one

the if statements check for bool data type in front of them , followed by a block of code {}
consider the example
~~~
  if a == b {
      do something
  }
~~~
if the return type of expression `a==b` is true the code block below will excute and skip otherwise
note the expression in front of if statement must always return bool not even Null, the code wont compile other wise
~~~
   if a = b {
    do something
}
~~~
the above code wont compile with error mismatched types expected bool, found ()
since the return type of `a=b` is null or `()` empty tuple is case of rust
the while loop works the same way as if but it loop until the statement in from of while does not return false
the loop statement does not have any statement in front of it, rather they are explictly break by the programmer using break in the loop block
consider this
~~~
loop {
   if some condition {
    break;
  }
}
~~~
rust does have for loop which can run on data types does impl iterator trait
~~~
for mut a in <some array>{
     a represents the elements in array
}
~~~
for loop also have python syntex

    for a in 10..=100 {}
the above code will run till `a <= 99`

match statement in rust works as match statement in other lanuage with syntex diffrent

    match a {
     10=>{ do something },
     20=>{ do something}
     _ => {}
     }  
in the above example we’re matching a with its value, if a equals to 10 the first block will execute , while if a equals to 20 then second, and if it does not equals to either of them then the third _ => block will execute
