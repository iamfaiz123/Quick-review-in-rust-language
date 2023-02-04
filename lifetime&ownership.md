# Owner ship in rust
rust unique approch to memory safty is obtained by rust owdership model.
 The in layman terms ownership model states that one a single variable at a given instance of time can have ownership to a certain memory location, ownership can be transfered from one variable to another but can never be owned by two or moreb variables at same time
 
 consider the example below
 ~~~
 fn main(){
     let a = "String".to_string();
     let b = a; // we have transfered the ownership of data which was stored by `a` to `b`
     let c = a; ///now if we try to assign the value of `a` to 'c' the compiler will give us                         an error since a does not hold and data init/// 
 }
 ~~~
  so what work around we can do ?
  A data location can only have one owner , but we can have multiple reference to that that location , or single mutable reference to that location.
  
in our previous example we can assign b and c refernce to a, wh
  ~~~
  fn main(){
     let a = "String".to_string();
     let b = &a; // b is referece to a
     let c = mut &a; // c is mutable reference to a
 }
 ~~~
 facts 
 `*` is called a dereferce operator in rust, using this we can always get the value behind and referece or `smart pointers`
 `smart pointers` are same as pointers in other language with some futures we will take about later.
  in the above example if we try to derefence the `&a` ie `*b` we will get the valye of a, but again we can change its value but can not assign it to something else.
  `let d = *b` would result in compilation error, since the owner of the data `b`'s pointing is `a` and b can not transfer data ownership.
  
  note - All the datatypes that does not implement the `Copy` trait can only have one owner at any given instance of time
  
  example 
  ~~~
  {
      let a:i32  = 10;
      let b = a;
      let d = a;
  }
  
  ~~~
  the above example would run fine since premitive data types like `i32` implements `Copy` trait, But if it was `String` or `custom data type` then it would have produce an error.
  you can implement `copy` trait for your own data types
  
  # function will always take ownership of data if not passed by refernce
  consider the example below
   
  ~~~ 
  {   
      fn callme( data: MyCustomData )->(){
          < some code >
      }
      fn main(){
          let A:MyCustomData = MyCustomData::new();
          callme(A); // the ownership of A's data has been given to data parameter in callme function
      }
      
      let c = A; //invalid operation , since the value of A has been moved
      }
~~~      
      

# quick question , Why we even need ownership and borrowing rule in rust?
Unlike other langauge rust does not have manual memory management or garbage collector , The Aim of rust is to provide `zero cost abstraction`. by saying this we dont have to worry about data race condition or memory leak , as any given instance of time only single variable can hold ownerhsip or single mutable refernce.

# life time in rust
Ok so you most of time you only need to pass your data references since you dont want to give ownership, But rust prohibits null pointers 
consider this
~~~
struct A{
    data: &MyCustomData  // the data field of struct A has a refernce to some other data of type `MyCustomData`
}
fn main(){
    let B:MyCustomData = MyCustomData::new();
    let c = A{
        data: &B
    }
   //if in near future if we drop the value of B, then the data being hold by C will become invalid. 
   //The above code wont compile as we need to assure rust that the data A is pointing will be valid this life time of A's object instance
}

~~~
`"The borrow checker uses explicit lifetime annotations to determine how long references should be valid. In cases where lifetimes are not elided, Rust requires explicit annotations to determine what the lifetime of a reference should be. The syntax for explicitly annotating a lifetime uses an apostrophe character as follows:"`
quick fact
when ever a varible goes out of scope, the `drop` method is called on that instance.You can yourself define `drop` trait for your own datatypes to do more work than just freeing upspace, `drop` is analogous to deconstructor in c++ . you can call drop trait at your will as well like `instance.drop()`

# how can we fix it then?
we need to tell the compiler explictly that the data which is being pointed by struct `A` is valid
~~~
struct A<`a>{
    data: &'a MyCustomData  
}
fn main(){
    let B:MyCustomData = MyCustomData::new();
    let c = A{
        data: &B
    }
   // the code will compile fine now
}
~~~
# what we did?
we created a generic struct with a lifetime parameter , which tell that the compiler that `'a` is a lifetime parameter.We assure compiler that we know the data will be valid .
life time parameters are always represented as `'a` or any other word but it must be prefixed with `'` , `'static` is a reserved keyword for lifetime in rust which tells the compiler the variable has lifetime till the program ends


  
  
 
 
