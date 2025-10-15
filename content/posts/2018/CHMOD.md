---
title: "Linux Chmod command explained"
date: 2018-08-29T00:00:00+05:45
draft: false
description: "Complete chmod tutorial: change file permissions in Linux using symbolic and octal notation. Master user, group, and other access controls."
tags: ["linux", "chmod", "file permissions", "unix", "command line", "system administration"]
cover:
    image: "https://i.imgur.com/LCV5cNA.png"
---
> Migrated from logicaletter.com


*CHMOD* aka *Change mode* is the command present in linux or unix like systems.
Purpose

To change the mode of a file for specified user classes according to specified permissions with different function.

Making it simpler it is used to change the permission of file . The basic syntax looks like

```
 chmod options permissions filename
```

## Permissions

Permissions are boundary for file access that can be defined for each user classes.

## User classes
Linux targets for multiple users on a single system. it is crucial to specify the classes of user so that the file is protected.The general classes of user are

- one who owns the file -“User”
- user who belong to file defined ownership group -“Group”
- other than user and group – “Other”

the mode or permission for file access for above user classes are

- Read “r” -the ability to look into the contents of file
- Write “w” -the ability to modify its content
- execute “x” -the ability to run the content of a file as a program

 
Lets create a file named *tutorial.pdf* using touch command


After creating the file. use ls -l comand

`ls -l`

![](https://i.imgur.com/nIldSYA.png)


on the left most side of tutorial.pdf you can see

`-rw-r--r--`

This is the permission for all user classes combined. The result may vary but let me break down this string with length of 10.

- The first ‘-‘ means this is a regular file
remaining 9 symbols are for Users , Group and others respectively where each one gets 3 symbols

Take a look at file permission presented above and compare it with this.

|Sn No 	|user class |	permission 	|permission description| 
|---|-------|-------|-----------|
|1 	|User 	|rw- 	|Can read and write|
|2 	|Group 	|r-- 	|can read and write|
|3 	|Others |r–- 	|can read only|
--------------------------------

 

chmod can be used to change the permission of user class.for example : if i want to change the permission of User to read ,write and execute then

### ` chmod u=rwx tutorial.pdf`


### if you want to change the mode for specific user classes then

- for user  “u”
- for group “g”

` chmod g=rwx tutorial.pdf `

### for others “o”
 `chmod o=rwx tutorial.pdf`

### Changing mode for all user classes

There are two ways that is going to be shown as example

` chmod u=rwx,g=rwx,o=rwx tutorial.pdf`

so what if you want to add rw for user and rx for group. you can perform that as well

`chmod u=rw,g=rx tutorial.pdf`

to set read , write and execute for all user classes.

`chmod =rwx tutorial.pdf`

Getting more technical

So briefing the symbolic mode as used in example we can update our syntax as

`chmod objects [ugo][+-=][permission] filename`

`Operators [+-=]`

The + operator adds new file access mode without destroying its previous file access mode. More like appending while file handling.

A real life example ” if you are trying to execute .sh file but permission is denied then you can use the following command

`chmod +x filename.sh`

The – operator  removes specified mode

If filename.sh has a read , write permission and if you want to remove write permission then

`chmod -w filename.sh`

the = operator more like destroying its previous mode and adding new modes

if filename.sh had read , write and execute permission and if you used this command

`chmod =w filename.sh`

Now you can only change the content of file. or your permission has been updated to write only.
 


 

Changing the mode of file access can also be defined by octal numbers also
|SN number |	Modes 	|octal representation|
|----------|-----------|--------------------|
|1 	|Read 	|4
|2 	|Write 	|2
|3 	|Execute 	|1
|4 |None 	|0

Three digit octal number represents modes for user classes user, group and owner.

let me clear that with an example : if you want read, write and execute for User then the 1st position value would be

`4+2+1 = 7`

for Group you want just read then

`4`

for others you want read and execute then

`4+1= 5`

overall the three digit number would bw

`745`

the command would be now

`chmod 745 somefile.sh`

These all represents the general use of chmod commands . for options you can refer to

### `chmod --help`