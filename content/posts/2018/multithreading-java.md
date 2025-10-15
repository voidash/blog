---
title: "The Art of Multithreading in java(Java programming guide)"
date: 2018-06-08T00:00:00-04:00
draft: false
description: "Master Java multithreading: create threads using Runnable interface and Thread class. Learn thread-based concurrency with practical examples."
tags: ["java", "multithreading", "concurrency", "threads", "programming", "runnable"]
cover:
    image: "https://www.thesprucecrafts.com/thmb/Kn0VEf6_rZ6OUj4XatYKQuCLgRk=/1883x1412/smart/filters:no_upscale()/thread-58169a705f9b581c0b669d89.jpg"
---
> Migrated from logicaletter.com 

Process Based Vs Thread Based

In your operating system , you are able to run many processes at a single time i.e you can scroll through your facebook messages in Google Chrome while watching new Avengers movie in VLC media player. This concept is process based thread model and Multi Threaded operating system can perform such tasks.If you want to learn more about operating system on the basis of processing , you can visit here. Here is important concept to thread model and that is thread based multithreading , just thread based which means Your program, if multi threading is enabled then can perform two or more tasks at a single time . i.e Suppose that your program has two buttons .First button your program performs  complex mathematical calculation and through second button it produces the sound of Cow. Your program tasks are performing mathematical calculations and producing cow sound .If your program is not multi threaded you won’t be able to do that on same CPU time.So what is Thread? Thread is dispatchable unit of code.
Java Thread Model

We have Single-Threaded systems and Multi-Threaded systems as opponents. Single Threaded systems use something called event loop with polling . It means infinite loop with polling which is event queue that decides what to do next. If one of your task is waiting for input from keyboard, until you won’t complete the input , no other task is going to be performed.

To counter that problem we have Multi-Threaded systems which basically removes that infinite loop  and polling mechanism is eliminated. How does it do that? It creates thread which can run independently on their own. The advantage of being independent is that even if one thread dies, other thread will continue doing its task.
Lets create a Thread

There are two basic ways to create  thread:

- Implement Runnable interface (Runnable is functional interface)
- Extend Thread class

Lets start with implementing Runnable interface

```java
public class newThread implements Runnable {
private Thread t;    
private int iteration;
//implementing Runnable means you will need a constructor that initializes the thread
    
    public newThread(String name,int iteration){
        t= new Thread(this, name ); //parameters are Runnable and String
        System.out.println("Executing "+name);
        this.iteration = iteration;
        t.start();  //starting a thread
    }
    public void run(){  //dispatchable code we talked about earlier is code inside run() method.
        try{
            for(int i =1;i<=iteration;i++){
                System.out.println("Child Thread "+ i);
                Thread.sleep(500); //500 is in milliseconds which is equivalent to 0.5 seconds
            }
        }catch(InterruptedException exc){
            System.out.println("something interrupted the thread");
        }
    }
}


//now the main Class ThreadDemo

public class ThreadDemo {

  
    public static void main(String[] args) {
        int iteration =10;
        new newThread("test #1",iteration);  
        
    
        
        
    }
    
}
```
If you implement Runnable functional interface then You will need a constructor.Here is the output of program
```
Executing test #1
Child Thread 1
Child Thread 2
Child Thread 3
Child Thread 4
Child Thread 5
Child Thread 6
Child Thread 7
Child Thread 8
Child Thread 9
Child Thread 10
```
With delay of 0.5 seconds until the iteration limit . dispatchable code prints Its iteration count.
Multithreading By extending Thread
```
public class newThread extends Thread {
    private int iteration;
    
    newThread(int iteration){ //constructor is optional 
        super("Test Thread");//can provide the name of thread too by accessing superclass constructor 
        System.out.println("Lets start");
        this.iteration = iteration;
        
    
    }
    public void run(){
        
        try{ //Thread.sleep() requires try-catch block as there might be interruption
        for(int i=1;i<=iteration;i++){    
            System.out.println("child thread " + i);
            Thread.sleep(500);  //500 milliseconds 0.5 seconds
        }
        }catch(InterruptedException exc){
            System.out.println(exc);
        }
    }
}

//now main class DemoThreadExtend

public class ThreadDemoExtend {

  
    public static void main(String[] args) {
        new newThread(10).start();
        
    }
    
}
```
If you extend Thread , there is no need of constructor but in the above program. Here constructor has been used to set thread name and getting value of iteration.

```
Let's start
Child Thread 1
Child Thread 2
Child Thread 3
Child Thread 4
Child Thread 5
Child Thread 6
Child Thread 7
Child Thread 8
Child Thread 9
Child Thread 10

With delay of 0.5 seconds until the iteration limit . dispatchable code prints Its iteration count.
```