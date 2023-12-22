---
title: "Writing SQL Parser in Rust"
date: 2023-12-23T00:05:48+05:45
draft: true
cover:
  image: "https://i.imgur.com/o7Nj8Rp.png"
---

## Context Free Grammar

You might have heard about syntax and Grammar before. The language we speak, the programming langauges we use; all of them follow certain rules of grammar. [Noam Chomsky](https://sbs.arizona.edu/chomsky/about) was the first one to algorithmize grammar and he called it "Context-Free-Grammar". It's the fule that defines how to generate pattern of strings. Let's start with English langauge, then we will build up towards SQL parsing.

```
<Sentence> -> <Article> <Noun> <Verb> <Article> <Noun>
<Article> -> "the"
<Noun> -> "cat" | "dog" | "ball"
<Verb> -> "chased" | "played with"
```
`<Sentence>` is our final form which is composed of `Article` + `Noun` + `Verb` + `Article` + `Noun`. The valid statements here are  

```
"the" ("cat" or "dog" or "ball")
("chased" or "played with") "the" ("cat" or "ball" or "dog")

    "the cat chased the cat"
    "the cat chased the dog"
    "the cat chased the ball"
    "the dog played with the cat"
    "the dog played with the dog"
    "the dog played with the ball"
    "the ball chased the cat"
    "the ball chased the dog"
    "the ball chased the ball"
    "the cat played with the cat"
```

What you just saw was [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) which is a code that expresses the syntax of formal languages. Lets look at another example that defines  a natural number


```
<digit excluding zero> -> "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
<digit>                -> "0" | <digit excluding zero> ;
<natural number>       -> <digit excluding zero> { <digit> };
```

A natural number starts from 1 so we defined `digit excluding zero` such that first digit is always a digit from 0 to 9. Zero in other position counts . i.e : 1000, 1024. In conclusion natural number is just a combination of `digit excluding zero` and **zero or more** `digit`. 


> In `<natural number> -> <digit exluding zero> { <digit> }` , the curly brackets based `{ <digit> }` means we can have zero or more number of digit, which means 10, 123, 2234, 234442, or any natural numbers all are validated by this expression.


## Tokens

A token is a sequence of character. The phrase `"the cat chased the dog"` has the following table 

| Token   | Token Type |
|---------|------------|
| the     | Article    |
| cat     | Noun       |
| chased  | Verb       |
| the     | Article    |
| dog     | Noun       |

The grammar which the tokenization is based from is

```
<Article> -> "the"
<Noun> -> "cat" | "dog" | "ball"
<Verb> -> "chased" | "played with"
```
So tokenization basically is converting words into tokens and mapping it according to grammar.

## Parsing

Parsing is process of analyzing the sequence of tokens, often in form of language's grammar to determine the grammatical structure. Let's switch the gears and now understand a basic SQL grammar. Note that this only covers `Select` statement however it can be extended very easily.


```
<SQLStatement>     -> <SelectStatement>
<SelectStatement>  -> SELECT [DISTINCT] <ColumnList> FROM <TableName> [WHERE <Condition>] [GROUP BY <GroupByList>] [ORDER BY <OrderByList>]

<ColumnList>       -> <ColumnName> [, <ColumnName>]*
<GroupByList>      -> <ColumnName> [, <ColumnName>]*
<OrderByList>      -> <ColumnName> [ASC | DESC] [, <ColumnName> [ASC | DESC]]*

<Condition>        -> <Expression> [AND <Expression>]*
<Expression>       -> <ColumnName> <Operator> <Value>

<TableName>        -> [a-zA-Z]
<ColumnName>       -> [a-zA-Z]
<Value>            -> {[0-9]}

<Operator>         -> = | != | < | > | <= | >=

```

Again lets touch the basics






SQL is database manipulating domain specific language.   

