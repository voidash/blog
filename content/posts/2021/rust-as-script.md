---
title: "Scripting with Rust: A basic Guide"
date: 2022-01-12T14:29:05+05:45
draft: false
description: "Learn how to use Rust for scripting tasks. Covers primitive Rust scripts with rustc, cargo-script for dependency management, and practical examples of file operations and testing."
cover:
    image: "https://i.imgur.com/Zsbk06w.png"
tags: ["Rust", "Scripting", "Programming", "Cargo", "CLI", "Tutorial"]
---

Was Rust built to be a scripting language? the types which you can use for crawling  web, extracting links, sending mails etc. Generally stuff that serve single functionality with limited scope. Python fits into that domain perfectly and it is pleasant to write scripts in python. But what about Rust? The language doesn't even come with a garbage collector, and you might as well fight its borrow checker until it deems your code is memory safe. When writing simple scripts, one might not need the features that rust guarantees. Moreover Rust doesn't run in a virtual machine like python does and Compiling scripts everytime you make a single change seems tedious to the workflow. But still you can use rust as a scripting language. Sure there will be more lines than that in python code but it might just make sense in some use cases. 


## A Primitive Rust Script
Without touching external crates, scripting is possible if you use shell such as fish, Zsh or bash to run those scripts. First use `rustc` to compile the program and ask it to produce its executable inside `/tmp` folder and then run that executable.

```rust
//bin/true; rustc -o "/tmp/$0.bin" "$0" && "/tmp/$0.bin" "$@"; exit $?
fn main() {
        println!("Hello, rust scripting.");
}
```

```
// to run the program
 $ chmod +x filename.rs
 $ ./filename.rs
```
- `/bin/true` is a command that returns exit status as `true`. Then why use it at all? It's because next command that you run doesn't require absolute path. Here in this case `rustc` doesn't require absolute path which might be `/home/user/.cargo/bin/rustc` or `/usr/bin/rustc` 
- `rustc -o /tmp/$0.bin "$0"` : `$0` is the first argument,the name of the file itself. the `-o` flag is the to set the output path to `/tmp` folder
- `&& /tmp/$0.bin "$@"`: if the compilation was successful then run the program. `$@` means get all the other arguments provided except the filename. For example: if the command you run is `./primitive.rs a b c` then, `a, b, c` are the arguments it will pass
- `exit $?`: `$?` means the last command that was run. In this case it's the execuable itself so we exit. 


For a simple enough program this might do the job but when it requires external dependencies such as `serde`, `actix-web` etc. There is a problem. This script doesn't allow usage of external crates and if you really want. You can use `-L` flag to pinpoint the crates. example : `rustc -L actix/build $0`. It really is a hassle. First clone the repo and build it, then point the build folder for `rustc`.

What if we could use `cargo` to download and manage the external dependencies. It's the program built for that purpose, plus what if we could add a test to our script. That would actually be awesome.

## Easy Life with  [`Cargo Scripts`](https://github.com/DanielKeep/cargo-script)

[Cargo scripts](https://github.com/DanielKeep/cargo-script) is the cargo subcommand for quickly writing rust scripts and is developed by Daniel Keep. 
install it with `cargo install cargo-script`. More information is in the repository `README.md` itself. Here is the sample script to list all the mp3 files in a given folder provided as argument. 

```rust
#! /usr/bin/env run-cargo-script 

//! ```cargo
//! [dependencies]
//!
//! glob = "0.3.0"
//!
//! ```

extern crate glob;

use glob::glob;
use std::env;

fn main(){
    let args: Vec<String> = env::args().collect();
    for entry in glob(format!("{}/**/*.png",args[1]).as_str()).unwrap() {
        println!("{}", entry.unwrap().display());
    }

}


#[cfg(test)]
mod test{
    use super::*;

    #[test]
    fn simple_test() {
        assert_eq!(4,4);
    }
}

```


To run the program simply provide `chmod +x`  permission to the file and it will list the mp3 files in folder recursively.

`./find-mp3.rs /home/cdjk/Downloads/`


To check if program passes test cases, you can use

`cargo script find-mp3.rs --test`


### Explanation

```
#! /usr/bin/env run-cargo-script 

//! ```cargo
//! [dependencies]
//!
//! glob = "0.3.0"
//!
//! ```

```
The first line is a [`shebang`](https://en.wikipedia.org/wiki/Shebang_(Unix)) which gives information about which program should the code be loaded to. For a bash script it is `#! /bin/bash`  , for a python script it might be `#! /bin/python` and for a cargo script it is `#!/usr/bin/env run-cargo-script`. `env` can be used to print environment variables or to launch programs with proper environment variables loaded in.

Other lines starting with `//!` will be included in Cargo.toml file and it is used to include the dependencies of the program. 

---------------

```rust
 for entry in glob(format!("{}/**/*.png",args[1]).as_str()).unwrap() {
        println!("{}", entry.unwrap().display());
    }
```
 [`glob`](https://en.wikipedia.org/wiki/Glob_(programming)) is simply a pattern to match multiple filenames using a series of wildcard characters such as `*`, `?` etc. `glob` crate is implmentation of unix like glob-matching for rust. In our program the glob was `/**/*.mp3` which means recursively search folders and files that ends with .mp3 extension.


----------------
```rust
use std::env;
let args: Vec<String> = env::args().collect();
```

`env` allows collecting arguments that is passed to the executable. 

---------------


Rust can be used for scripting, it might not be practical for all usecases but when one requires it, this example can be a starting point as it covers some basic features that scripting demands.







