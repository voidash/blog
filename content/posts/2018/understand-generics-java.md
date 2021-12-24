---
title: "What are Generics in Java"
date: 2018-06-02T00:00:00+05:45 
draft: false
cover:
    image: "https://media.istockphoto.com/vectors/blueprint-of-building-vector-id510230824?k=20&m=510230824&s=612x612&w=0&h=r7Ja-6GUzK5QVumR79ZhLiXACHRg9oDmbQ_fH5ZGvVk="
---
> Migrated from logicaletter.com 


Lets start why generics is needed rather than identifying what generics really are. Through the use of generics one can create classes,interfaces and methods and specify what kind of data it is working upon. Lets start up with example : Many algorithms logic is same no matter what kind of data they are working upon.  Stack algorithm logic is same  no matter which type of data you are working with ,i.e String , Integer etc; the basic functionality of algorithm remains same.Once you apply generics to stack algorithm , you can smoothly work with variety of data types. i.e if you want to create stack of Strings you can create it or if you want to create stack of Integers , no one is stopping you. If you are not familiar with stack algorithm then you can look here . Lets get back into generics.
So what are generics?

Generics are Parameterized types. What the heck are TYPES? Classes, Arrays, Interface, primitive everything are types.  Parameterized types  are important because it allows us to create classes, interfaces and methods in which the type of data they operate upon is specified as parameter. Sometimes importance of something carries meaning with it. If you understand the importance of Generics , you understand its meaning.

Lets get straight into example
```java
class Gene<dataType> {
    //objectname is identifier and dataType is placeholder 
    dataType objectName; 
    //creating a constructor that accepts dataType reference type.
    Gene(dataType parameterObjectName){  

   //local variable gets the value of parameterized variable
       objectName = parameterObjectName;  
    }
    
    dataType getObjectValue(){
        return objectName;
       
    }
    
}
```

```
public class GenDemo {
    public static void main(String[] args){
//creating instance of class Gene
       Gene<Integer> objectInstance =new Gene<Integer>(2); 
/*
Gene<Integer> objecInstance2 = new Gene<Integer>("this is wrong"); 
will not compile because Integer reference type 
doesn't support String reference type
*/
        //returns 2 from the method getObjectValue defined above
        System.out.println(objectInstance.getObjectValue()); 
        
    }
```

So lets examine the above code. take a look at `Gene<dataType>` .  dataType  is actually parameter for reference data type. Type parameters are declared within angular brackets `< >`.  Now Gene is parameterized type as from the defination of Generics.

`dataType objectName;`

dataType is placeholder for actual type that will be passed later.For example if String is passed then dataType will hold String. Lets peek into the method getObjectValue.
```
dataType getObjectValue(){
return objectName;}
```

 

Type parameter can be used to specify the return type of the method as in getObjectValue method. Lets get into GenDemo class.

```Gene<Integer> objectInstance =new Gene<Integer>(2);```

Notice how Gene class instance is created with Integer as a type argument that is passed to Gene’s type parameter.<Integer> where Integer is inside angular brackets.

 

So if i create Gene’s instance  , is different versions of Gene created? No compiler does not create different version of Gene . Rather compiler specifies the cast for type argument where ever needed.

## Advantage of Generics
You may ask “Generics might be cool and stuff but how does it compare against my non generic code?” .In a nutshell

- Fixing compile time errors is better than run time errors. Use of generics means compiler issues error if your program violates type safety more easily .Is it there too much terminologies ? let me break it down. Compile time is the time when your code is being converted into computer understandable form. Run time means the time when your program is actually running on your Primary memory .i.e RAM. Type safe means code that can only access memory location that it is authorized to. Type safe code cannot perform operation on object it is not authorized to. Type safety means compiler will check the type of variable it is assigned and returns error if wrong type of variable is assigned.For example:

```
    String s = 1; // not correct ,trying to put integer into string
    int i = "se"; //fails too , trying to put string to integer
```
- No more casting in your java code. You are creating a ArrayList and adding string into it. Now you want to access your string on location 1, then you will need to convert it into String first
```
    List list = new ArrayList();
    list.add("hello");
    String s = (String) list.get(0); //casting is used.
```

But if you use generics
```java
    List<String> list = new ArrayList<String>();
    list.add("hello");
    String s = list.get(0);   // no cast
```