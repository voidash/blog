---
title: "Alias Linux Command Explained succintly"
date: 2018-12-29T00:00:00+05:45
draft: false
description: "Create custom Linux command shortcuts with alias. Learn to substitute strings for commands and boost your terminal productivity efficiently."
tags: ["linux", "alias", "command line", "bash", "terminal", "productivity"]
cover:
    image: ""
---
> Migrated from logicaletter.com

## Purpose

> to substitute a String in place of commands used in linux. Note: commands can include paramaters too.

- If you are familar with CMD then you would certainly know cls clears the screen. but in linux shell there is command called clear to wipe out terminal junk. so if i want to use cls in linux or unix like OS terminals then i would use

`$ alias cls='clear'`

Next time if i want my screen cleared i can use

`cls`

- if i want to create a file where owner can only read,write or execute the file then .to set file access permission before creating a file

`alias owneronly = 'umask u=rwx,g=,o= '`

then i can create a file with that specific file mode. if i use owneronly


- pwd displays the current directory we are in. so instead of typing pwd i can just type p by setting

`alias p='echo $pwd'`

- if i want to prevent accidental deletion of files then i can set

`alias rm ='rm -i'`

rm -i basically means ask before deleting.