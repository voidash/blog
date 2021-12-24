---
title: "9's complement Method: Computers use those?"
date: 2017-03-12T00:00:00+05:45
draft: false
cover:
    image: "https://media.istockphoto.com/vectors/blueprint-of-building-vector-id510230824?k=20&m=510230824&s=612x612&w=0&h=r7Ja-6GUzK5QVumR79ZhLiXACHRg9oDmbQ_fH5ZGvVk="
---

> Migrated from logicaletter.com 

computers are intricate to understand at the superficial level, but at its core all you will see are logic. Logic makes everything. The invention of transistors allowed us to create logic gates and those logic gates allow computers to add. couple of XOR gates and AND gates can create a full adder. but how do computers subtract? they use something called 1’s complement because computer developers thought “screw this , we are not going to build another circuit for subtraction, rather we will use same circuit board that we use for addition”. They achieved their word by using something called complement method.

### so whats complement method?? Here is simple example:

> Subtract 233 from 2342.

its pretty straightforward for us , the answer is 2342-233=2109 but for computer

-  Step 1: specify subtrahend and minuend2342 (minuend)

```
233 (subtrahend)
```

- step 2: if there are not equal number of digits make equal digits on both minuend and subtrahend

```
    2342 (minuend)
    0233 (subtrahend)
```
- step 3: find 9’s complement  of subtrahend

```
    9's complement = 9999-0233 = 9766

    step 4-add 9’s complement of subtrahend and minuend

     2342(minuend)
    +9766(subtrahend)
    _____
    12108
```

- Step 4: if there are more digits on sum than subtrahend or minuend, in our case 12108. here minuend and subtrahend are 4 digit numbers whereas our sum is 5 digit so its left most digit is carry here in 12108 , 1 is carry so will write it as 1/2108

>if there is carry digit then :
>add the carry digit

        (1)2108 //here (1) is carry digit so
        2108
          +1
        2109 //answer

> NOTE:  
>if there is no carry digit then:
> again find its the complement of its sum and it will be the answer but in negative

so this is how computer takes your data and subtract it , but computer understand only binary language aka the language of 0 and 1. On next post i will be talking about 1’s complement method
