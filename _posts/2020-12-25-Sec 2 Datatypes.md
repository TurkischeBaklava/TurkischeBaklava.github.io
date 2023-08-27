---
title: Data Types And Case Expression
date: 2020-12-26 00:35:29 +/-TTTT
categories: [Programming Languages, Section 2]
tags: [pl part a] 
---
A programming language should categorize its variables into some types at some point during the life cycle of a program.
We can categorize the types into two as _base types_ and _compound types_.
Programming languages have base types, like int, bool ,char and 
compound types, which are types
contain other types in their definition, like list, tuple, options etc.

To create a compound type, there are really only three essential building blocks. 
Any decent programming language provides these building blocks in some way.

<br/>

**Each-of (also known as product types):** A compound type t
describes values that contain each of values of type t1, t2, ..., and tn.

**One-of (also known as sum types):** A compound type t
describes values that contain a value of one of the types t1, t2, ..., or tn.

**Self-reference (also known as recursive types):** A compound type t may refer to itself in its definition in order to describe recursive
data structures like lists and trees.
<br/>

<br/>

### **Examples**

Tuples build each-of types
- _int * bool_ contains an int and a bool

Options build one-of types
- _int option_ contains an int or it contains no data

Lists use all three building blocks
- _int list_ contains an int and another int list or it contains no data

<br/>

## Datatype Bindings


```ml
datatype mytype = TwoInts of int * int
				| Str of string
				| Kebab
```

* Adds a new type mytype to the environment
* Adds constructors to the environment: TwoInts, Str, and Kebab
* A constructor is (among other things), a function that makes
values of the new type (or is a value of the new type):
	* TwoInts : **int * int -> mytype**
	* Str : **string -> mytype**
	* Kebab : **mytype**

<br/>

## How can we access to datatype values: Case Expression

<br/>

``` ml
fun f x = (* f has type mytype -> string *)
	case x of
	Kebab => "Doner Kebab"
	| Str s => s
	| TwoInts (x,y) => (Int.toString x) ^ (Int.toString y) (* Here pattern matching is like a let expression: It binds variables in a local scope.*)
```

_Each branch has the form **p => e** where p is a pattern and e is an expression._

<br/>

## Some Useful Examples of "One-of" Types
<br/>

Enumarting a fixed set of options:


```ml
datatype suit = Club | Diamond | Heart | Spade
datatype rank = Jack | Queen | King | Ace | Num of int 

(* As it seen in the code, Unlike C and Java, ML lets variants carry data*)
```
We can then combine the two pieces with an each-of type: `type cards = suit * rank`

<br/>

## Another useful example of one-of types:

<br/>

>Suppose you wan to indetify students by their id-number, but in case there are students
that do not have one, then you will use their full name instead.

>This datatype captures the idea directly:

```ml
datatype id = StudentNum of int
		 	| Name of string * (string option) * string
```
<br/>

Above in the code, a student only can have either a number or name. Which is perfectly proper
approach for the one-of types. 
	However most people insist on using each-of types which is considired bad code:
```ml
	(* If student_num is -1, then use the other fields, otherwise ignore other fields *)
	{student_num : int, first : string, middle : string option, last : string}
```
On the other hand, each-of types are exactly the right approach if we want to store for each student their
id-number (if they have one) and their full name:
```ml
{ student_num : int option,
  first       : string,
  middle      : string option,
  last        : string }
```
<br/>

Our last example is a data definition for arithmetic expressions containing constants, negations, additions,
and multiplications.

```ml
datatype exp = Constant of int
			 | Negate of exp
			 | Add of exp * exp
			 | Multiply of exp * exp
```

Thanks to the self-reference, what this data definition really describes is trees where the leaves are integers
and the internal nodes are either negations with one child, additions with two children or multiplications with two children.


We can write a function that takes an exp and evaluates it:
```ml
fun eval e =
	case e of
		Constant i => i
	  	| Negate e2 => ~ (eval e2)
		| Add(e1,e2) => (eval e1) + (eval e2)
		| Multiply(e1,e2) => (eval e1) * (eval e2)
```
_Functions over recursive datatypes are usually recursive._

>So this function call evaluates to 15:
`eval (Add (Constant 19, Negate (Constant 4)))`

<br/>

## List and Options are Datatypes 

<br/>

_Linked list of integers:_
<br/>

```ml
datatype my_int_list = Empty
			| Cons of int * my_int_list

val one_two_three = Cons(1,Cons(2,Cons(3,Empty)))	

fun append_lists (xs,ys) = 
	case xs of
	Empty => ys
	| Cons(x,xs') => Cons(x,append_lists(xs',ys))	
```

_Int option datatype:_

```ml 
datatype int_option = NONE | SOME of int
 ```

<br/>

## Pattern-matching on list type

<br/> 

**[]** really is a constructor that
carries nothing and **::** really is a constructor that carries two things, but :: is unusual because it is an _infix_
operator (it is placed between its two operands), both when creating things and in patterns:

```ml
fun sum_list xs =
	case xs of
	[] => 0
	| x::xs' => x + sum_list xs'

fun append (xs,ys) =
	case xs of
	[] => ys
	| x::xs' => x :: append(xs',ys)
```
<br/>


## Polymorphic Datatypes
<br/>

The only thing that distinguishes the built-in lists and options
from our example datatype bindings is that the built-in ones are polymorphic - they can be used for carrying
values of any type. 

You can do this for your own datatype bindings too, 
and indeed it is very useful for building "generic" data structures.

**This is exactly how options are pre-defined in the environment:**

```ml 
datatype 'a option = NONE | SOME of 'a 
```

Such a binding does not introduce a type option. Rather, it makes it so that if t is a type, then t option is type.
