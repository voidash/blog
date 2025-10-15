---
title: "Types of Operating systems on basis of processing"
date: 2019-05-21T00:00:00+05:45
draft: false
description: "Explore different types of operating systems based on process handling: batch, time-slice, multithreading, embedded, and real-time systems."
tags: ["operating systems", "multitasking", "multiprogramming", "multithreading", "real-time", "embedded systems"]
cover:
    image: "https://i.imgur.com/Mc37EmH.png"
---
> Migrated from logicaletter.com 



Operating system is system software that manages the resources of computer, one that directly talks to your hardware. When your computer boots up , operating system is loaded into your main memory and and it starts performing tasks. These tasks are processes for your computer and depending upon the task you may have one or many processes for a single task. To have a better understanding, if you are on Windows , Take a look at Task manager and you will find a Running processes tab. This may give you a better look at  processes that are currently active. Operating system can be classified under  how it handles these processes category and listing them down we will have :

- Batch processing operating system
- Time slice operating system
    - Multi programming operating system
    - Multi Tasking operating system
- Multi threaded operating system
- Embedded operating system
- Real time operating system

## Batch processing operating system

You will never have to use those operating system which handles processes as batches but once someone had to use it. Back in the days of second generation computers, Batch processing based operating system were very popular. So how did it work?Suppose you had four task to be performed in this arrangement ADD,SUBTRACT,ADD, SUBTRACT. Here is a problem , Operating system has to switch between its tasks four times but if similar task ADD, ADD and SUBTRACT,SUBTRACT are grouped together as batches  then it has to switch between tasks for only two times instead of four times. That’s how batch processing operating system works.

## Time slice operating system

According to processing power of your computer, amount of cycles it can perform in 1 seconds may vary. Cycles per second basically means  ON and OFF  that your computer can perform at one second. if your computer has processing power of 3 GHz , it means your computer can perform 3 billion cycles of switching in one second. But how is it related to Time slice operating system? If your computer can perform 3 billion cycles in one second , it does not mean that it has to perform that amount of switching every time. Sometimes it may stay idle waiting for input too. It depends upon the task and the your operating system  capabilities. Time slicing operating system slices the time and gives CPU time to each processes according to priority order or on average scale. It is important to understand that in Time slice operating system ,the tasks are not running concurrently. Time slice operating system can be classified into  Multi programming and Multi tasking.

## Multi programming operating system


Imagine you have a computer program that takes input from the user and finds the sum of the given input numbers. When your Computer is demanding input , at that moment CPU may remain idle without doing anything so Multi programming operating system handles such  situation by providing CPU time to another processes which computer can run at that time period maximizing the CPU efficiency. Remember that for two process does not run simultaneously rather than that Two processes are loaded into memory but one is executed at a time and until it is not finished, another process doesn’t get CPU time.

## Multi Tasking operating system

Imagine that there  are 15 customers  in the supermarket and one employee. All the 15 customers bags is needed to be filled with grocery items they just bought. If it takes 1 second to load one item into bag for a employee , then he gives 1 second for every customers. To load one item into bag for each customers every time he/she has to switch to other bags every 1 second; therefore he/she gives equal amount of his time so that he can load bag of all customers by switching to others customers bag. The philosophy of Multi Tasking operating is also same. All it does is divide the CPU time for each processes and switch back and forth between processes such that each processes gets served the same CPU time. It also slices time but there is key difference between multi tasking and Multi programming. While in multi programming , process gets CPU time until it finishes its job but in multi tasking, process gets equal but smallest CPU time and it switches back and forth between other processes even though the job of process is not complete.
Multi Threaded operating system

The ability to run word processor software like Microsoft word and browser such as Google chrome at a same time is example of multi threaded operating system. It performs this operation by dividing processes into Threads. Thread is smallest dispatchable code that can be run concurrently. In Multi threaded operating system  process can run concurrently which basically means more than two processes can run at a same CPU time. Unlike Multi programming and multi tasking , it doesn’t have to switch between processes rather it just runs at a same time.
Embedded operating system

Can you feel technology around you? if your answer is yes then you are familiar with technologies that can even automate your microwave oven , your gate which automatically opens when you are near, the traffic light system that changes its color every minute. Have you ever thought how it achieves its task? It uses its embedded operating system so that it can perform its task.

## Real time operating system

Imagine you have to design hand robots for car manufacturing, where timing is the key factor.Your hand robot should have multiple features. for example it should cool some generic engine on the industry by blowing air and When car starts to come towards the hand robot, it has to perform some specific task such as screwing the nuts or joining metal parts. Based upon priority it should be able to switch into into the task it should perform and it has to be accurate with respect to time, not even a single micro second delay otherwise cars may come out faulty. In such case scenario Real time operating system handles the control of machinery. Real time operating system can be divided into  Hard Real time operating system and soft real time operating system. Soft real time operating system can take some delay but hard real time operating system takes precise steps to perform task at exact given time, even if other operation is already running in its system.
