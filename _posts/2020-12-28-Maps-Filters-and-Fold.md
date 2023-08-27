---
title: Map, Filter And Fold
date: 2020-12-28 01:10:29 +/-TTTT
categories: [Programming Languages, Section 3]
tags: [pl part a]  
---

## Map

``` ml
(* Type of map: ('a -> 'b) * 'a list -> 'b list *)

fun map (f,xs) = 
	case xs of
	[] => []
	|x::xs'	=> (f x)::map(f,xs')
```
The map function takes a list and a function f and produces a new list by applying f
to each element of the list.

```ml
val incremented = map ((fn x => x + 1),[1,2,3,4]) = [2,3,4,5]
```

We could have easily written a recursive function over lists of integers that incremented all the elements, but
instead we divided the work into two parts: 
The map _implementer_ knew how to traverse a recursive data
structure, in this case a list. 
The map _client_ knew what to do with the data there, in this case increment each
number.

<br/>

## Filter
```ml
(* Type of filter: ('a -> bool) * 'a list -> 'a list *)

fun filter (f,xs) = 
	case xs of
 	[] => []
	| x::xs' => if f x 
			then x :: filter (f,xs')
			else filter (f,xs')	
```
The filter function takes a list and a function f and produces 
a new list containing only the elements of the input list for which the function
returns true.
```ml
fun get_all_even_snd xs = filter ((fn (_,v) => v mod 2 = 0), xs)
```
<br/>

## Fold
```ml
(* Type of fold: ('a * 'b -> 'a) * 'a * 'b list -> 'a *)

fun fold (f,acc,xs) =
	case xs of
	[] => acc
	|x::xs' => fold (f, f(acc,x), xs')
```
The fold function takes an "initial answer" acc and uses f to "combine" acc and the first element of the list, using this
as the new "initial answer" for "folding" over the rest of the list. We can use fold to take care of iterating
over a list while we provide some function that expresses how to combine elements.
```ml
fun sum_lst xs = fold ((fn (x,y) => x + y), 0, xs)
```
