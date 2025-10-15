---
title: "Introducing Nushell"
date: 2022-01-29T20:07:44+05:45
draft: false
description: "Introduction to Nushell, a modern cross-platform shell written in Rust that treats everything as structured data. Compare traditional Bash workflows with Nushell's intuitive table-based approach."
tags: ["Nushell", "Shell", "Rust", "CLI", "Terminal", "Linux", "Structured Data"]
---

At it's core Shells are just interactive user interface that exposes OS tools and services. This blog is a introduction to **Nushell** , a cross-platform shell written in Rust . We already have had fair share of shells like Bash, Zsh, Fish etc. Why do we need yet another shell? All existing shells have one thing in common,   They all treat as if **everything is a text**. Enter **Nushell** where **Everything is a structured data**. 

## Bash and Nushell : Head to Head

### Bash

lets say **you need to list directories only in bash**.

`ls -l | grep ^d` will do the trick. How does this work? When you `ls` and look at each line, The line with directories will always have `d` present as a first character. Something like this
```
drwxr-xr-x  8 cdjk cdjk      4096 Jan 24 18:44  Documents
|
┴── First character is d

```

So we list all the contents in current directory then pipe it to `grep` command which looks for all the *lines* in a piped **text** that starts with `d`. Now that is not bad for a bash script. 

**what if we want to get all the directories , then get its size and later list only those that are bigger than 1 gigabyte**. Try doing that using `ls` and `grep`.

For this scenario one option is to  `du -sm * | awk '$1 > 1000'` where `du -sm *` gets the size and name of files present in the current directory seperated by space. The output without the awk part will be 
```
331	Desktop
209	Devanagari-To-Braille-DTB
1	develop
20887	Documents
11734	Downloads
```


Awk is a mini excel basically. From the `awk` perspective the above output by `du` has two fields : `$1` and `$2` seperated by space. *First* field or `$1` is numeric. i.e `331`, `209`. It is actually a size of the file present in *second* field. i.e `Desktop`, `Documents` 

`awk '$1 > 1000'` will compare the first field or size that's bigger than 1000mb and print only those lines that meet the criteria. So when piped together to form `du -sm * | awk '$1 > 1000'` This will list the contents with size bigger than 1000mb.


Those two tasks of *listing directory only* and *getting directory with size bigger than 1000mb* look similar but we used 4 tools. For first tak we used `ls` and  `grep`. For second task we used `du` and `awk`. It's really a burden when scripting in bash due to sheer amount of tools and types of workflows present in bash. my thought process while scripting in bash is first google, then look at man pages to fit the criteria in a loop until whole script is complete and this process is really tiresome. 

### Nushell
**Nushell** is different and intuitive. To demostrate that Lets perform the same task in nushell.

`ls | where type == Dir | each { du $it.name } | where physical > 1000mb`. 

Doesn't it look comprehensive than the bash counterpart. Isn't it intuitive? and all in all we are just using  commands `ls` and `du`.

Here is the whole process dissected for better understanding plus some features about nushell. 
- First list the contents in directory using `ls` 
```
───┬──────────────────────┬──────┬──────────┬──────────────
 # │         name         │ type │   size   │   modified   
───┼──────────────────────┼──────┼──────────┼──────────────
 0 │ 2022-01-24_18-44.png │ File │ 281.0 KB │ 5 days ago   
 1 │ asset                │ Dir  │   4.1 KB │ 2 months ago 
 2 │ course 4th Sem       │ Dir  │   4.1 KB │ 2 weeks ago  
 3 │ games                │ Dir  │   4.1 KB │ a month ago  
 4 │ github               │ Dir  │   4.1 KB │ 3 hours ago  
 5 │ java                 │ Dir  │   4.1 KB │ a month ago  
 6 │ joplin               │ Dir  │   4.1 KB │ 2 months ago 
───┴──────────────────────┴──────┴──────────┴──────────────
```

Wait why is there a table? Everything is structured data in nushell. This table is actually stuctured data and is interactive. we can convert this table to **json** using `ls | to json`, or convert to **csv** using `ls | to csv`. if we  don't know what type of conversion is possible so we can even `ls | to --help` to get all the possible conversion formats.

- Second, we use `where` subcommand for filtering table to match the condition. If you look again at the table above there is a `type` column and its contents are either `File` or `Dir`.  so we can use `ls | where type == Dir` for filtering out the directory.

```
ls | where type == Dir
            |       |
         column   unique value in column    
```

```
───┬────────────────┬──────┬────────┬──────────────
 # │      name      │ type │  size  │   modified   
───┼────────────────┼──────┼────────┼──────────────
 0 │ asset          │ Dir  │ 4.1 KB │ 2 months ago 
 1 │ course 4th Sem │ Dir  │ 4.1 KB │ 2 weeks ago  
 2 │ games          │ Dir  │ 4.1 KB │ a month ago  
 3 │ github         │ Dir  │ 4.1 KB │ 4 hours ago  
 4 │ java           │ Dir  │ 4.1 KB │ a month ago  
 5 │ joplin         │ Dir  │ 4.1 KB │ 2 months ago 
───┴────────────────┴──────┴────────┴──────────────
```

> What if we want all the unique values present in  `type` column. Since nushell has dataframes, we can leverage it to get the unique types. We convert our table to dataframe then select the  `type` column and then list all the unique values present in the column. The whole thing looks like  
>
> `ls | dataframe to-df | $in.type | dataframe unique`

- Third, We will iterate through each row using `each` and for each row we will retrieve its name then check the size with `du $it.name`. 

`$it` is a variable that stores row that is currently pointed during iteration process. 

```
───┬────────────────┬──────────┬──────────┬─────────────────┬
 # │      path      │ apparent │ physical │   directories   │ 
───┼────────────────┼──────────┼──────────┼─────────────────┼
 0 │ asset          │  19.9 MB │  19.9 MB │                 │       
 1 │ course 4th Sem │   3.7 MB │   3.7 MB │ [table 1 rows]  │       
 2 │ games          │  15.0 KB │  98.3 KB │                 │       
 3 │ github         │  23.1 GB │  24.5 GB │ [table 47 rows] │       
 4 │ java           │ 225.6 KB │ 380.9 KB │ [table 4 rows]  │       
 5 │ joplin         │ 212.0 KB │ 368.6 KB │ [table 8 rows]  │       
───┴────────────────┴──────────┴──────────┴─────────────────┴
```
 We again get a table with columns `path`, `apparent`, and `physical`. 

- Fourth, We will get all the rows whose `physical column` values are greater than 1000mb with 

```
ls | where type == Dir | each { du $it.name } | where physical > 1000mb

───┬────────┬──────────┬──────────┬─────────────────┬───────
 # │  path  │ apparent │ physical │   directories   │ files 
───┼────────┼──────────┼──────────┼─────────────────┼───────
 0 │ github │  23.1 GB │  24.5 GB │ [table 47 rows] │       
───┴────────┴──────────┴──────────┴─────────────────┴───────
```


## Fetching Quotes from internet in nushell

I was looking through [public-api](https://github.com/public-apis/public-apis) list and found this gem [themotivate365](https://api.themotivate365.com/stoic-quote) that serves random stoic quotes.

Here is the random quote retrieved by Curl
```
> curl -L https://api.themotivate365.com/stoic-quote

{"data":{"author":"Nassim Nicholas Taleb","quote":"Weak men act to satisfy their needs, stronger men their duties."}}⏎
```

So we get JSON as a response. With nushell this is a cakewalk as structured data is its first class citizen.

```
> fetch "https://api.themotivate365.com/stoic-quote"
───┬────────────────────
 # │        data        
───┼────────────────────
 0 │ [row author quote] 
───┴────────────────────

```

As you can see the json is already parsed and converted to table. Now to get the quote we can use 
```
fetch "https://api.themotivate365.com/stoic-quote" | get data.quote
```

This shows how flexible is it to work with structured data actually in nushell. here are some of the cool oneliners that i randomly grabbed from discord nushell community.

jon#6709 : 

Query Wolfram Alpha
```
def w [...query] {
    let appID = # Get one at https://products.wolframalpha.com/api/
    let queryString = ($query | str collect ' ')
    let result = (fetch ("https://api.wolframalpha.com/v1/result?" + ([[appid i]; [$appID $queryString]] | to url)))
    $result + ""
}
```





