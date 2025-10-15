---
title: " Understanding inheritance in java : SuperclassVariable can reference a Subclass Variable"
date: 2018-10-23T00:00:00+05:45
draft: false
description: "Understand Java inheritance: how superclass variables reference subclass objects. Learn polymorphism with practical parent-child class examples."
tags: ["java", "inheritance", "OOP", "polymorphism", "subclass", "superclass"]
cover:
    image: ""
---
> Migrated from logicaletter.com 


Superclass : A parent class Subclass : A child class that inherits Superclass
```
    class A{    //superclass or parent class
    //some generic code
    }
    class B extends A{ //subclass or child class of A
    //some generic code 
    }
```

it may seem esoteric at first, but its true that superclass variable can reference a subclass object .This aspect is quite useful when superclass object needs parameters same as subclass object which is not part of that superclass object. Let’s take a example , Suppose Parent A is eligible for being a parent but does not have a child. Child A is child of Parent B . Parent A can have genes of Parent B , but not all of Child A. This statement is true in java . Here is the code to prove the statement.


> parentclass.java
```java
 class parentClass {
   private String name;
   private String initials;
    
    parentClass(String name ,String initials){
        this.name = name;
        this.initials = initials;
        System.out.println("che ");
        
        
    }
    parentClass(){
        name="n/a";
        initials="n/a";
    }
    String show(){
        return name+" "+initials;
        
    }
}
```
> 2.childclass.java
```java
class childclass extends parentClass  {
    private int age;
    childclass(String name,String initials,int age){
       super(name,initials);
       this.age = age;
    }
    public int showAge(){
        return age;
        
    }
}
```

> 3.InheritanceExample.java
```java
public class InheritanceExample {

 public static void main(String[] args){
     childclass child = new childclass("barsha","BT",7);
     parentClass Parent =new parentClass();
     System.out.println(Parent.show());
     System.out.println(child.showAge());
     
     //till this point the output will be n/a n/a and 7as there is no parameter passed for parentClass
     //Now lets try something new like Superclass referencing subClass
     
     Parent = child;
     System.out.println(Parent.show()); // this will be valid as show is the part of Parent class 
     //the output will be barsha BT
     //System.out.println(Parent.showAge()); this wont be valid as showAge is not the part of superclass
     //actually Parent does not know anything about its child i.e what is happening within it so child part will not be transferred.
     
            
 }    
}

```

 - Take a look at inheritanceExample.java.
    - When Superclass references Subclass it is important to remember that it is the type of reference variable-not the type of objects that it refers to
    - that determines what members can be accessed. Superclass A can take reference from Superclass B through Subclass B but it can’t access any part of Subclass B. That is the reason why `showAge()` can’t be accessed by Parent instance, `System.out.println(Parent.showAge());` I hope it is making sense since i have no or very less traffic when i am writing this so when i review my blog i hope i understand what i am writing right now. 