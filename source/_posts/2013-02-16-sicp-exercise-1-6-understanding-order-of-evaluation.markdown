---
layout: post
title: "SICP Exercise 1-6 : Understanding order of evaluation"
date: 2013-02-16 20:05
comments: true
categories: sicp lisp
---

SICP Exercise 1.6 is very interesting. I found it difficult to understand the problem and then the solution. It was fun to actually understand and reason about the solution.

In section 1.1.7 we define a function to calculate square root of a number by Newton's method of successive approximation.

<!-- more -->

``` scheme Newton's method of successive approximation for finding square root of a number

(define (square x)
  (* x x))

(define (average x y)
  (/ (+ x y) 2))

(define (improve guess x)
  (average guess (/ x guess)))

(define (good-enough? guess x)
  (< (abs 
    (- x (square guess))) 0.0000000001))

(define (square-root-iter guess x)
  (if (good-enough? guess x)
    guess
    (square-root-iter (improve guess x) x)))
```

Exercise 1.6 defines a replacement for special form `if` as follows


``` scheme 'if' as a ordinary procedure

(define (new-if predicate then-clause else-clause)
  (cond (predicate then-clause)
    (else else-clause)))
```

Now `square-root-iter` function changed as follows

``` scheme New version of 'square-root-iter'

(define (square-root-iter guess x)
  (new-if (good-enough? guess x)
    guess
    (square-root-iter (improve guess x) x)))
```

The problem asks us, what is the result of this version of
`square-root-iter`?

Definition of `new-if` is correct and it can be tested with some test
cases.

``` scheme 

(new-if (= 5 0) 0 5)
; 5
(new-if (= 0 0) 0 0)
; 0
```

But still evaluation of `square-root-iter` with `new-if` results in
infinite loop. Why?

> The answer is `order of evaluation`.

In scheme, there is a general rule of evaluation. First value inside
parenthesis is considered as function and all other values are passed
as arguments to that function. Only `Special forms` are evaluated
differently.

`if` is a special form which is evaluated in following way.

    (if <predicate> <consequent> <alternative>)

> First, <predicate\> is evaluated. If it is true, then <consequent\> is evaluated. Otherwise <alternative\> is evaluated. `But both <consequent> and <alternative> are never evaluated at the same time.`


Our `new-if` is not a special form. So it will be evaluated as any
other function following `applicative order of evaluation`.

Applicative order of evaluation first evaluates all operands
completely and then passed them to operator. There is also `normal
order evaluation` in which operands are evaluated `when they are
actually needed`.

Scheme interpreter uses applicative order. So in new version of
`square-root-iter`, while evaluating `new-if` it will try to evaluate
it's operands first. But the second operand of `new-if` is recursive call to
`square-root-iter`. So this evaluation will never finish.

Problem is not in implementation of `new-if` but in it's `evaluation`.
