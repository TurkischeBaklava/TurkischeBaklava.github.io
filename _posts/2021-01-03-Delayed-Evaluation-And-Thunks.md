---
title: Delayed Evaluation and Thunks
date: 2021-01-03 01:29:29 +/-TTTT
   

---


## Thunks Delay

In Racket Language, any expression that we wrap with parenthesis can call itself with no
argument. Since it's not a special form (define, if, begin), it will be treated as a function.

When we do this: `(1)`

We are treating "1" like a function, it expects its argument, however we get this bad error:

	application: not a procedure;
 	expected a procedure that can be applied to arguments
  	given: 1
  	arguments...: [none]

(e1 e2 e3 e4) =>  e1 is a function that has arguments: e2, e3, e4
So parenthesis in Racket MATTER ! We can not use them as we can use in other languages for operation priority.

<br/>

## Thunking !

 We know how to delay evaluation: out expressions in a function !

   *  Thanks to closures, can use all the same variables later

 A zero-argument function used to delay evaluation is called a thunk

   * As a verb: thunk the expresison

<br/>


 Evaluate an expression "e" to get a result:
 
 `e`

<br/>

 A function that when called, evaluates e and returns result
   * Zero-argument function for "thunking"

` (lambda () e)`

<br/>

 Evaluate e to some thunk and then call the thunk

` (e)`

<br/>


## Avoid unnecessary/expensive computations 

<br/>


Thunks let you skip expensive computations if they are not needed

**Great if you take the true-branch:**
``` scheme
(define (f th)
	(if (...) 0 (... (th) ...)))
```

Above in the code we defined a function called **f** that takes a **thunk**.
Imagine the thunk does not worth to compute as we called the function. Because
that's how it works: Arguments of a function are evaluated even before the funtion itself
called. We need this expression until we need it! Whenever "if" comes to be #f. That's good
move when we need the thunk once in the program.

<br/>


**But worse if you end up using the thunk more than once:**
``` scheme
(define (f th)
   (... (if (...) 0 (... (th) ...))
        (if (...) 0 (... (th) ...))
        ...
        (if (...) 0 (... (th) ...))))
 ```

We could easily get in trouble here. In case we might need the thunk for several times,
it is gonna be evaluated for several times. This is even worse than one unnecessary/expensive computation.

<br/>


``` scheme
(define (slow-add x y)
  (letrec ([slow-id (lambda (y z)
                      (if (= 0 z)
                          y
                          (slow-id y (- z 1))))])
    (+ (slow-id x 50000000) y)))


(define (my-mult x y-thunk)
  (cond [(= x 0) 0]                                    
        [(= x 1) (y-thunk)]                             
        [#t (+ (y-thunk) (my-mult (- x 1) y-thunk))]))

```

> (x = 0) case branch: We did not evaluated the expression, great !

> (x = 1) case branch: We did evaluated the expression once, we done what we have to do. OK !

> #t	case branch: The shit hits the fan. We just did not evaluated the expression. We did
evaluated it more than once, whenever recursion occurs. However we could have use the same
expression where it is needed.

<br/>


So let's remember the results of the thunk for later executions:

``` scheme
;(my-mult n (let [x (lambda () (slow-add 2 4)] (lambda () x)))

(my-mult 0 (let ([x (slow-add 3 4)]) (lambda () x)))

(my-mult 1 (let ([x (slow-add 3 4)]) (lambda () x)))

(my-mult 20 (let ([x (slow-add 3 4)]) (lambda () x)))
```

We cut off the repeated calculations in any case by doing so. We evaluated it once, and 
use it as we need. 
Trade off: We gave up the (x = 0) case being fast.

<br/>


## Best of both worlds

- not compute it untill needed
- remember the answer so future uses complete immediately

That's called **Lazy Evaluation...**

<br/>


Suppose we have a large computation that we know how to perform but we do not know if we need to
perform it. Other parts of the program know where the result of the computation is needed and there may
be 0, 1, or more different places. If we thunk, then we may repeat the large computation many times. But if
we do not thunk, then we will perform the large computation even if we do not need to. To get the "best of
both worlds," we can use a programming idiom known by a few different (and perhaps technically slightly
different) names: lazy-evaluation, call-by-need, promises. The idea is to use mutation to remember the result
from the first time we use the thunk so that we do not need to use the thunk again.

One simple implementation in Racket would be:

```scheme

(define (my-delay f)
	(mcons #f f))

(define (my-force th)
	(if (mcar th)
	(mcdr th)
	(begin (set-mcar! th #t)
		(set-mcdr! th ((mcdr th)))
		(mcdr th))))


(my-mult 0 (let ([p (my-delay (lambda () (slow-add 3 4)))])
             (lambda () (my-force p))))

(my-mult 5 (let ([p (my-delay (lambda () (slow-add 3 4)))])
             (lambda () (my-force p))))


```














