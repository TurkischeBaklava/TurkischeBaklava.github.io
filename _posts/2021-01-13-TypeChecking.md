---
title: Type Checking
date: 2021-01-13 00:35:29 +/-TTTT
categories: [Programming Languages, Section 7]
tags: [pl part b] 
---

## Type Checking 

The process of verifying and enforcing the constraints of types—type checking—may occur either at **compile time** (a static check) or at **run-time** (a dynamic checking). If a language specification requires its typing rules strongly (i.e., more or less allowing only those automatic type conversions that do not lose information), one can refer to the process as strongly typed, if not, as weakly typed. The terms are not usually used in a strict sense.



## What is Static Type Checking ?

Static type checking is the process of verifying the **type safety** of a program based on analysis of a program's text (source code). What is usually meant by _"static checking"_ is anything done to reject a program after it parses but before it runs. If a program does not parse, we still get an error, bu we call such an error a "syntax error" or "parsing error".

In contrast, an error from static checking, typically a "type error" would include things like undefined variables or using a number instead of a pair.
We do static checking without any input to the program identified - it is "compile-time checking" though it is irrelevant whether the language implementation will use a compiler or an interpreter after static checking succeeds.

If a program passes a static type checker, then the program is guaranteed to satisfy some set of type safety properties for all possible inputs.

The most common way to define a language’s static checking is via a **type system**. In ML,gave typing rules for each language construct: Each variable had a type, the two branches of a conditional must have the same type, etc. ML’s static checking is checking that these rules are followed (and in ML’s case, inferring types to do so). But this is the language’s approach to static checking _(how it does it)_, which
is different from the purpose of static checking _(what it accomplishes)_. The purpose is to reject programs that “make no sense” or “may try to misuse a language feature.” 



## What is Dynamic Type Checking ?

Dynamic type checking is the process of verifying the type safety of a program at runtime. Implementations of dynamically type-checked languages generally associate each runtime object with a type tag (i.e., a reference to a type) containing its type information. This runtime type information (RTTI) can also be used to implement _dynamic dispatch, late binding, downcasting, reflection,_ and similar features.


By definition, dynamic type checking may cause a program to fail at runtime. In some programming languages, it is possible to anticipate and recover from these failures. In others, type-checking errors are considered fatal.

Programming languages that include dynamic type checking but not static type checking are often called "dynamically typed programming languages". 




