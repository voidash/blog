---
title: "Hacking Casio Calculator"
date: 2023-12-29T15:15:25+05:45
draft: true
cover:
  image: "img/2023-12-29-15-38-14.png"
---

Let's hack the fx-991es calculator. Precisely we are exploiting the Buffer Overflow technique and doing some stuff {TODO}.Let me be clear that the calculator is a computer anyways. Buckle up as weare going to dive deep into some Computer Architecture.

### Basics of fx-991es

Don't get afraid of fancy words or fancy figures. After all they are the simple chain of concepts hidden behind some characters. 


- **Chip**: nX-u8 Series chip. <a href="./assets/nx-u8 manual.pdf" >Manual Here</a>
- **Architecture**: [Super Harvard](https://en.wikipedia.org/wiki/Super_Harvard_Architecture_Single-Chip_Computer)

### CPU Resources and Programming Model : In Gajju We Trust

#### General Architecture

![](img/2023-12-29-16-03-18.png)

Straight out of manual, we have two address spaces: 512KB for code space and 16MB for data space. 
All the spaces are divided into physical segment of 64 kilobytes. 

#### Registers

![](img/2023-12-29-16-04-30.png)

We care only about **Program Counter** and **Link Registers**.

![](img/2023-12-29-16-18-39.png)
Program counter holds the address for the next instruction to execute. 

![](img/2023-12-29-16-17-07.png)
Link Registers saves the contents of the program counter (PC) during subroutines/function calls and Interrupts. We only care about the first one called **LR** which is for Subroutine Call.

## Memory Model Revised
Since this is harvard architecture, you have different space for code and different space for memory. The code space word length is 16bit. The data memory word length is 8bits

The Memory map we are concerned with is Data Memory which contains ROM and RAM. According to [casio calc wikidot](https://casiocalc.wikidot.com/memory-map) the memory map is as follows

> The notation is as Follows `segment: hex_value`

- ### ROM : `0:0x0000h - 0:0x7FFFh`
Rom is not important as we can't access it anyways.

- ### RAM : `0:0x8000h 0:0x8DFFh` 
    - **Input Area**: `[0x8154 .. 0x81B8]` (total of 100 bytes)

    Input Area stores whatever you type in calculator, from numbers to formulaes. 

    - **Cache Area(Buffer Area)**: `[0x81B8 .. 0x821C]` (total of 100 bytes)

    Cache Area stores the previous buffer if you press AC so that you can go backgward with `Replay Button`, the big circle one on center near the screen

## Understanding the Vulnerability 


## Big Spoiler

You can't directly program it.... well, all these architecture shenanigans were there because we are **ROPPING(Return Oriented Programming)** 

![](img/2023-12-29-17-18-01.png)

-----------
-----------

### What is Return Oriented Programming
**ROP(Return Oriented Programming)** is not related to the image above. 

#### Stack
Let's get started


