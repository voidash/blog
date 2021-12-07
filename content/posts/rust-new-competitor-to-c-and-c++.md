---
title: "Rust New Competitor to C and C++"
date: 2021-12-07T17:20:48+05:45
description: ""
draft: false
ShowReadingTime: true
cover:
    image: "https://www.incredibuild.com/wp-content/uploads/2021/09/Rust_vs_C_800x533.jpg"
---
This post tries to reason why Rust might be choice for new projects instead of C and C++. This is a bold statement and there is a need to discuss the types of projects where a comparision is being made. i.e Systems Programming.  Going back a bit further, and taking a look at the motivation for these languages shines a beam of light about the main goal of these languages.

# C and C++ 
![](https://semioticitamati.in/assets/img/clientlogo/cc.jpg)
C first appeared 49 years ago which addressed cross platform system programming issues. It allowed programmers to maintain a single codebase for multiple machines with different instruction sets that existed in that era. C is older than the 8085 processor but to this day it allows a great deal of flexibility in writing code. Mainly thanks to its subsequent releases that added in new features . It has proved its capability through projects such as large projects such Linux, chromium, MySQL etc. 
 C++ appeared 36 years ago mainly as an extension to C. C++ was referred to as C with classes. Object oriented paradigm, exceptions and other key features that were lacking on C were added while providing backward compatibility to C. This meant programmers could try out OOP patterns,better handle exceptions and modularize the codebase. 
On github(code hosting provider), there are 40,516 public repositories for C++ and 40,212 public repositories for C. They seem to be equally matched and are great tools. They both allow low level control over the data through direct access to memory, heap allocation etc.They can also be compiled to machine level code directly  and have excellent syntax . Programming languages like Java,  C# are inspired by C and C++ syntax.  C++ is modern. The community recently released c++20 standard. It has all the bells and whistles like metaprogramming through the use of templates, RAII memory management which ensures memory safety through the use of Smart pointers, and a huge collection of modern libraries for a myriad of tasks. The community is active. If a project based on C and C++ is to be started and if one follows the guidelines it will allow a maintainable codebase. 

# What makes C and C++ languages good for systems programming? 
![](https://www.corelis.com/wp-content/uploads/2019/10/insystem-programming-.jpg)
-  Efficient memory management .
-  Low level control over flow of program.
-  No overhead to run the programs which makes the size of the executable very small.
-  Compilers available for a multitude of architectures like ARM, RISC-V, intel , AMD , Atmel AVR etc. 
-  Compiler options and optimizations are available
-  availability of massive number of libraries  
 
# Where does C and C++ fail?
There are a set of standards and guidelines a programmer must follow in order to make their program run without crashes or without any security holes. They also have to learn fine details of toolchain which requires years of experience and possibly someone to mentor them. Programming languages like C and C++ doesn't enforce any guidelines to how programs should be written. They aren't opinionated. For most cases it is fine and it’s up to programmers to enforce guidelines on their programs. But following guidelines requires proper tooling and might be a hectic task. It is one of the reasons why bugs are prevalent in any codebase. Programs written in  C and C++, are prone to memory unsafety. Memory unsafe programs have undefined behaviour. And are vulnerable to various exploits such as Buffer overflow. Buffer overflow allows malicious parties to execute unintended pieces of code or get access to confidential information easily. And since C and C++ supports multiple architectures even in the embedded system realm. The hazards are very fearsome. On top of The growth of embedded systems such as the Internet of Things(IOT has been rapid. If they are programmed without safety in mind then they will be left vulnerable. Moreover, Real Time embedded systems like Air Traffic control systems, Command control systems can cause catastrophic damage if security holes are left behind. 

# Rust
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRUsOlQ30aRfgPcaGBK4T8YNcxqOHqJlN-PJA&usqp=CAU#center)
 C++ and C doesn't enforce any form of guidelines or enforce safety but there is one new programming language that does it. Rust can be used in place of  C and C++ because it allows dealing with low level details like memory management, data representation and concurrency. Rust has been voted most loved programming language for straight six years in a stackoverflow developer survey. There must be a good reason for Rust to be voted as the most loved programming language and E. Dunham, operation engineer at mozilla puts the most feature of Rust succinctly, "To write code free of certain classes of bugs, in any language, you need to follow a bunch of rules. And what sets Rust apart is that these rules live explicitly in the language specification and in the compiler". 
 
# But what is Rust exactly? 
![](https://miro.medium.com/max/1200/1*ESXbarxuve9lSFfh6Je65g.png)
Rust is a multi paradigm, general purpose programming language that is designed for performance, safety and concurrency.  Rust can be written in imperative paradigm like C , declarative paradigm like Haskell , and supports actor communicator paradigm like erlang. Rust has very small overhead so it's executable size is low, it has speed comparable to C and C++ and has concise and readable syntax. It deals with low level details like memory management, data representation and concurrency very well. Rust follows fail-fast design. In terms of memory safety this means If there is a possibility of dangling pointer, if there is an uninitialized pointer or  if there is a possibility of memory leak then the compiler won't let you compile the program. Type safety on Rust makes sure the program doesn't treat float like int and Thread safety allows for more robust code for concurrency based problems. The tools that come bundled with rust are so powerful it actually reduces bugs through its linguistic features and libraries . For example: Testing on rust is much easier than that on C or C++ as rust supports unit tests and integration tests out of the box. Rust provides access to its documentation through rustup which can be viewed even offline and it has a very powerful package manager called Cargo . Adding new dependencies on the project is seamless like in python and Javascript.
 
# Rust: More than system programming
With Rust one can Program Operating systems, web browsers, web protocols,  tools for embedded systems or program embedded systems itself. Rust is much more than a C++ substitute. There is Django for python, Rails for Ruby but there is no good web framework for C or C++. Even though the tools used to deliver such apps are built on C++ and C , one can't take as much time in building web applications as the time it takes to build  projects such as web browsers and OSes.  C and C++ tools are hectic , development time with C and C++ is significantly larger. Basically you trade minimal overhead and blazing fast execution with longer development time. But, Rust is flexible. Certainly its primary goal is not to replace scripting languages like Javascript but like Django for python there is Rocket for Rust(Rocket is a framework for building backend portion of web applications like Django)
Rust also has excellent support for webassembly which allows programs written in Rust can be compiled to wasm format that can be used by modern browsers to handle process intensive tasks at near native speed. 
 
# Who should invest time in Rust?
IOT enabled devices usage is increasing at a rapid pace and will keep continuing. A single loophole on IOT devices that might leave users vulnerable is enough to cause a big disaster. To prevent attacks such as buffer overflow, to prevent thread locking etc  Rust guarantees Memory , Type and thread safety so companies are adopting Rust for embedded systems programming. If one is interested in embedded systems, Rust might be a worthy tool to pick up. 

Software as a Service(SaaS) products are mainly available on browsers for users. Browsers seem to use a combination of markup language like HTML and scripting languages such as Javascript. But sometimes it is not ideal to use  javascript and its framework ,especially for  computationally intensive tasks  like photo rendering and for such tasks Web Assembly seems to be the option. WASM(Web assembly) is a type of code that can be run by browsers. It can perform complex calculations and processing at near native speed like C++ and C and that is not feasible with javascript. And Rust has the best support for the web assembly ecosystem. So even web developers sooner or later might need to fiddle with Rust when web assembly becomes more popular and mainstream services start using it. 

# Where To Learn Rust? 
rust-lang.org/learn has a book that explains concepts very well. Once you catch the basics, there is a code practice platform, exercism.org where you can find Rust track and practice various problems. 

