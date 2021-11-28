---
title: "Time and Linux"
date: 2021-11-28T08:23:51+05:45
draft: false
---


## Is dual boot messing up your date and time?
**Universal Time** or UTC is primary time standard by which the world regulates clock and time. GMT is timezone and UTC is time standard. 

**RTC Time** is run by your computer even when you don't power the device through the use of CMOS battery. It is also known as hardware clock. 

**System Clock** is maintained by operating system 

The time that UNIX type system stores on hardware clock is UTC
The time that Windows type system stores on hardware clock is system clock time. 


### Fix  : Make Linux store local time on RTC or hardware clock.

```
$ timedatectl set-local-rtc 1
```

## What if CMOS Battery is not present or it requires change?

Every time i boot and try to open up my browser and start surfing. I get the error 

```The clock on your system is behind. please update the clock```


## Fix : Simple Script that sends request to API and sets the date

- First send request to API using Wget
```$ wget -qO- "http://worldtimeapi.org/api/timezone/asia/kathmandu"  ```
    - Options used
        - `-q` : quiet , disables wget resolving and connecting portion output
        - `-O-` :Output set to terminal. send response to terminal instead of outputting to file
    - worldtimeapi.org was used. to specify your timezone change asia and kathamandu to your repective region and city.

ouput : 

```json
{"abbreviation":"+0545",
"client_ip":"27.34.22.231","datetime":"2021-11-28T19:47:05.812646+05:45","day_of_week":0,
"day_of_year":332,
"dst":false,
"dst_from":null,
"dst_offset":0,"dst_until":null,"raw_offset":20700,"timezone":"Asia/Kathmandu","unixtime":1638108125,"utc_datetime":"2021-11-28T14:02:05.812646+00:00","utc_offset":"+05:45","week_number":47}
```

- So the output is JSON format. we can parse JSON too but easier way will be to use `cut` command while setting delimetter to comma(,). delimeter specifies how to split the string. if we set it to comma then string will be seperated and stored in seperate field everytime comma occurs in the string. The field can be thought as element present in internal array.  we can access the elements in that internal array through `-f` or `--fields` option. We are interested on `unix_datetime` which occurs on 12th position so our command will be `cut -d "," -f 12`. But we have to pipe it to our first command so that first command's output can be used as input for `cut` command.

```bash
$ wget -qO- "http://worldtimeapi.org/api/timezone/asia/kathmandu" | cut -d "," -f 12

output:
"unixtime":1638108815
```

- we just need the numeric part . To remove it we can use `grep`. 

### The concept of LookAhead And LookBehind

#### LookAhead 
syntax: `(?<=Pattern)`

lookahead is used in scenario where there is a pattern that exists before the pattern we want to match.
If we want to match all `banana` that appears before the text `yellow` then you can use it as

```
$ grep -Po "(?<=yellow )banana"
```
Your exact match will be banana which starts with yellow.

#### LookBehind
syntax: `(?=Pattern)`

lookbehind is used in scenario where there is a pattern that exists before the pattern we want to match.
Suppose a scenario where you want to match all the `guns` that ends with `roses` then you can lookbehind as

```
$ grep -Po "guns(?=roses)"
```
the match will be `guns` only and not roses. 

- Option `-P` means use perl pattern matching.
- Option `-o` means show exact match only.


combining all together. 

```
$ wget -qO- "http://worldtimeapi.org/api/timezone/asia/kathmandu" | cut -d "," -f 12 | grep -Po "(?<=:).+"
```

- `(?<=:).+` since our previous output was `"unixtime":1638108815` and we want only numeric part we start by selecting `:` as a lookahead pattern and everything that is behind as a match. `.+` means select everything that is one or more till new line.


### Final command

- we will use `date` command with `--set` option to set new date. 
- If we want to use unix timestamp then we have to include `@` on front of stamp.

```
$ sudo date -s '@'$(wget -qO- "http://worldtimeapi.org/api/timezone/asia/kathmandu" | cut -d "," -f 12 | grep -Po "(?<=:).+")
```


