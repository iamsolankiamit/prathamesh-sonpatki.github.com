---
layout: post
title: "SICP Exercise 1-11"
date: 2013-03-10 20:53
comments: true
categories: sicp recursion itertive-solution
---

##### Problem -

A function f is defined by the rule that

f(n) = n if n < 3 and

f(n) = f(n-1) + 2 * f(n-2) + 3 * f(n-3) if n >= 3.

Write a procedure that computes f by means of a recursive process.

Write a procedure that computes f by means of an iterative process.

<!-- more -->

##### Solution -

`Recursive solutions is very simple.`

``` scheme Recursive solution
    (define (f n)
        (if (< n 3)
            n
            (+ (f (- n 1)) (* 2 (f (- n 2))) (* 3 (f (- n 3))))))
```

`Iterative solution is a bit different.`

Base condition is same for iterative solution also. If n < 3, n is the
answer.

For n >= 3, we have to define a procedure which will evolve as a
iterative process.

For n >= 3, `f(n) = f(n-1) + 2 * f(n-2) + 3 * f(n-3)`

For n = 3, we have to evaluate above rule once. For n = 4, we have to
evaluate it twice. First to find out f(3) which then will be used to
find f(4).

So, it is clear that for n >= 3, we have to evaluate this rule `n - 2`
times. Lets name diff to (n - 2).

Now lets consider state variables which will hold all the information
to find solution at a particular instance in this process.

For n = 3, we need f(0), f(1) and f(2) to find out f(3).
They are respectively 0, 1 and 2.

Lets name them a = 0, b = 1 and c = 2.

Now answer in each iteration is `3 * a + 2 * b + c`

This answer needs to be passed to recursive call till diff is 0.

So after each iteration, state variables are changed as follows:

`b becomes a`

`c becomes b`

`3 * a + 2 * b + c becomes c`

`diff becomes diff - 1`

For n = 3, initially, a = 0, b = 1, c = 2 and diff = 3 - 2 = 1,

After one iteration, a = 1, b = 2, c = 4 and diff = 0

Now, base condition is when diff = 0, c is the answer. So here for
f(3), 4 is the answer.

Lets repeat this for f(4)

n > 3

diff = 4 - 2 = 2

a = 0, b = 1, c = 2

After first iteration,

diff = 2 - 1 = 1

a = 1, b = 2, c = (3 * 0 + 2 * 1 + 2) = 4

diff > 0 So second iteration will be called

After second iteration,

diff = 1 - 1 = 0

a = 2, b = 4, c = (3 * 1 + 2 * 2 + 4) = 11

Now diff = 0, so c is the answer.

f(4) = 11

The implementation of this process is recursive as shown below. But
the `process which is evolved is iterative.`

``` scheme iterative-solution

    (define (f n)
        (if (< n 3)
            n
            (f-iter 0 1 2 (- n 2))))

    (define (f-iter a b c diff)
        (if (= diff 0)
            c
            (f-iter b c (+ (* 3 a) (* 2 b) c) (- diff 1))))
```

Iterative process evloved for f(4)

``` scheme

    ;; (f 4)
    ;; (f-iter 0 1 2 2)
    ;; (f-iter 1 2 4 1)
    ;; (f-iter 2 4 11 0)
    ;; 11

```
