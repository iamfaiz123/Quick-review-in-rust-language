# Package managemnt in Rust
Cargo is the Rust package manager. Cargo downloads your Rust package's dependencies, compiles your packages, makes distributable packages, and uploads them to crates.io, the Rust community's package registry
Cargo is like npm to node.js

# creating a new project using cargo
on termial write `cargo new <project name>`
this command will generate a file named as your project with 3 contents
~~~
src
src/main.rs
cargo.toml
~~~
the binary entry point of your code lives in `main.rs`
execute the command `cargo run` to run the code
by default `cargo run` command will run the code is unoptimized way, it is sutible for development purpose
`cargo build --release` will build the application will all compiler optimization, this  will build a optimized executable for your code. located in `target/release`.

you will notice when you compile your code using `cargo run` or `cargo build` a file name `cargo.lock` will also get generated, which will contains the imformation about the current version of dependencies that your project relies onto.

on `cargo.toml` file is all your external dependencies will be added.
you can add dependecies using `cargo add <dependencies name>` or by manually adding into `cargo.toml` file

here is an example of manually adding the dependencies
~~~
  [dependencies]
  serde_json = {version = "1.0.0" features = "full"}
~~~

# some important cargo commands
* `cargo fmt` to format your rust code. you can change your design of code format by adding `rustfmt.toml` file next to `src`
* For linting in rust , rust have `clippy`. You can install clippy as binanry using `cargo install clippy` . use `cargo clippy --fix` to lint your code
* use `cargo check` to check for any erros in your code without compiling

# Importing libraries modules in code
`use <crate_name>::<modules>` to import modules from external dependencies that you added into `cargo.toml`
to use your own codes defined into other files use `mod <folder or file name>`.
 example
 ~~~ 
 src/
    main.rs
    bin/
       module1.rs
       module2.rs
       mod.rs
cargo.toml
cargo.lock
~~~
`mod.rs` imports the modules defined in the folder, when ever we import a module in rust using `mod <module name>` rust compiler will check for a file name with the same module or will look for a folder with same name which has `mod.rs` init


## The Module Setup
Consider a generic `cargo new` – generated project with one module we want to expose. We will have the following participating files and folders named in a very generic way here. These are listed from deep inside the project tree to higher levels:

`file1.rs`, `file2.rs`,... that contain code we want to expose
public functions and structs are written here using the keyword pub at the beginning of their declaration
macros come with their `#[macro_export]` anyways, so no further changes needed
a directory `src/folder` that contains the files from 1.
a file src/folder.rs that lists all files within folder that we want to expose by doing:
~~~
pub mod file1;
pub mod file2;
…
~~~
our `main.rs` (or `lib.rs`, depending of your app design) that references the exposed code by doing:
~~~
mod folder;
use crate::folder::file1::pub_function1
use crate::folder::file1::pub_function2
use crate::folder::file2::pub_function1
use crate::folder::file2::pub_function2
~~~


