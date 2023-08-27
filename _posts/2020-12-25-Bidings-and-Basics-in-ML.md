---
title: Bindings, Syntax, Type-Checking and Evaulation Rules in SML
date: 2020-12-25 01:29:29 +/-TTTT
categories: [Programming Languages, Section 1]
tags: [pl part a]     
---

### Expressions And Variable Bindings in ML

An ML program is a sequence of bindings. Each bindings gets type-checked and then (assuming it type-checks) evaluated.

Let's consider variable binding now:

`val x = e` 

Here, val is a *keyword*, x can be any *variable*, and e can be any *expression*. This could be a simple ML program. A __value__ is an expression that "has no more computation to do".  

In programming languages there are two terms that actualize the program when we are hitting keyboard:

1. Syntax

* Syntax is just how you write something. Without knowing syntax of the language you will just typing some words which is kinda gibberish.

2. Semantics

* Semantics is what that something mean. 
    * Type-checking (before program runs)
    * Evaluation (as program runs)

The implementation of the language is doing when it sees all this code:

``` ml
    val x = 7;
    
    (*static environment: x : int*)
    (*dynamic environment: x --> 7*)
    

    val y = 15;
    (*static environment: x : int , y : int*)
    (*dynamic environment: x --> 7, y --> 15*)
    
    val z = (x + y) + (x * 2)
    (*static environment: x : int , y : int, z : int*)
    (*dynamic environment: x --> 7, y --> 15, z --> 36*)

```

In ML program, whenever we bind a variable some value, type-checking system gives a type to that variable in static environment before the program runs. At a certain line of the code, a static environment is roughly the types of the preceding bindings in the file. As the variable type-checks, program can run and evaluate the static environment into dynamic environment where all the variables hold a value.

Simply:  

_static environment (for type-checking subsequent bindings)_  
_dynamic environment (for evaluating subsequent bindings)_

In the above example: for `val z`, when type-checker looked at this expression, it looked up and saw those two variables which have type int and similarly because that's an addition expression it gave the __z__ type int in the static environment. As the program evaluates dynamic enviroment placed the corresponding values. So static environment acts a lot like the dynamic except what's the type, what's defined or what's not. First everything type-checks, then if that passes, everything runs.

So far,

* A program is a sequence of bindings
* Type-check each binding in order using the static environment
produced by the previous bindings
* Evaluate each binding in order using the dynamic environment
produced by the previous bindings
    * Dynamic environment holds values, the results of evaluating
expressions
* All values are expressions
* Not all expressions are values
* A value “evaluates to itself” in “zero steps”

### Whenever you learn a new construct in a programming language, you should ask these three questions: What is the syntax? What are the type-checking rules? What are the evaluation rules ?