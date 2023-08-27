---
title: Tail Call Optimization
date: 2020-12-26 01:19:29 +/-TTTT
categories: [Programming Languages, Section 2]
tags: [pl part a] 
---

One of the characteristic features of functional programming is recursion concept.
Recursions are not suppose to be harder than using loops. Actually often much easier than loop:

* When processing a tree
* Avoids mutation, even for local variables

Nevertheless sometimes recurions might be less efficient rather than loops. Then how to reason
about efficiency of recursion ?

<br/>

## Call-stacks

<br/>

In computer science, a call stack is a stack data structure that
stores information about the active subroutines of a computer program. 
A call stack is used for several related purposes, 
but the main reason for having one is to keep track of the point to which 
each active subroutine should return control when it finishes executing.
Such activations of subroutines may be nested to any level (recursive as a special case), 
hence the stack structure.

While a program runs, there is a call stack of function calls that
have started but not yet returned

* Calling a function f pushes an instance of f on the stack
* When a call to f finishes, it is popped from the stack

<br/>


## Tail-call optimization

<br/>

Tail-call optimization is where you are able to avoid allocating a new stack frame for a function 
because the calling function will simply return the value that it gets from the called function.
We can use the constant stack space when we take advantage of tail-call optimization for the recursive function.

```ml
(* Factorial function with recursive call*)
fun fact n = if n=0 then 1 else n*fact(n-1)

val x = fact 3

(*
Evaluation steps

fact 3
3 * (fact 2)
3 * (2 * (fact 1))
3 * (2 * (1 * fact 0))                       
3 * (2 * (1 * 1))
3 * (2 * 1)
3 * 2
6

*)

(* Factorial function with tail recursion *)

fun fact n = 
	let
	   fun aux (n,acc)
			if n = 0
			then acc
			else aux(n-1,acc*n)
	in
	   aux(n,1)
	end

val x = fact 3

(*
Evaluation steps

fact 3
aux (3,1)
aux (2,3)
aux (1,6)
aux (0,6)
6

*)

```
