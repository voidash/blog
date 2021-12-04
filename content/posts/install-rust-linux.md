---
title: "How to install rust on linux"
date: 2021-12-04T08:23:51+05:45
draft: false
---

This post discusses how to install rust on arch and debian based distro. 

## Build Essential
Rust doesn't include [linker](https://en.wikipedia.org/wiki/Linker_(computing)) (which is required for generating execuatable ) on its installer. 
To install the linker,  first check the type of distro you have 
```
$ uname -a 
Linux cdjk 5.13.13-1-MANJARO 1 SMP PREEMPT Thu Aug 26 20:34:21 UTC 2021 x86_64 GNU/Linux
```

Mine is manjaro which is arch based system. But i have provided instruction for both arch and debian based systems. 

- for arch based distro : `sudo pacman -S base-devel`
- for debian based distro: `sudo apt install build-essential`


## perform system update
- for arch based distro : `sudo pacman -Syy`
- for debian based distro: `sudo apt update`


## install Rust
you can head to the [rust-lang.org/tools/install](https://www.rust-lang.org/tools/install) and grab the curl command for rustup or just copy 

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

follow the instructions then after installation check
```$ cargo -v ```
```$ rustc -v ```

If version number shows then your installation was successful. 
