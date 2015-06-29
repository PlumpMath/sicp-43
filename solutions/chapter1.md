### 1.1

```
-> 10
10
```

```
-> (+ 5 3 4)
12
```

```
> (- 9 1)
8
```

```
> (/ 6 2)
3
```

```
-> (+ (* 2 4) (- 4 6))
6
```

```
-> (define a 3)
-> (define b (+ a 1))
a is 3
b is 4
```

```
-> (+ a b (* a b))
19
```

```
-> (= a b)
#f
```


```
-> (if (and (> b a) (< b (* a b)))
	b 
	a)
4
```

```
-> (cond ((= a 4) 6)
	  	 ((= b 4) (+ 6 7 a))
	  	 (else 25))
16
```

```
-> (+ 2 (if (> b a) b a))
6
```

```
-> (* (cond ((> a b) a) 
       	    ((< a b) b)
            (else -1)) 
   (+ a 1))
16
```


### 1.2


```lisp
-> (/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5)))))
   (* 3 (- 6 2) (- 2 7)))
-37/150
```

### 1.3

```lisp
(define (square a) (* a a))
(define (square-sum a b) (+ (square a) (square b)))
(define (largest-square-sum a b c)
  (cond ((< a b) (if (< a c) (square-sum b c) (square-sum a b)))
        (else (if (< b c) (square-sum a c) (square-sum a b)))))
```


### 1.4

Explain:

```lisp
(define (a-plus-abs-b a b) 
  ((if (> b 0) + -) a b))
```

Answer:

We choose the operator based on the predicate value of the value `b` being
greater than 0 or not. If `b` is positive the operator result is `+` otherwise
it's `-`.  So our operator is the result of the compound expression, which is
then used for adding or subtracting to the operands `a` and `b`.


### 1.5

We have a test procedure below. 

```lisp
(define (p) (p)) 
(define (test x y)
  (if (= x 0) 0 y))
```

Evaluate the expression:

```lisp
(test 0 (p))
```

What behaviour will we observe with an interpreter that uses applicative-order evaluation?
What behaviour will we observe with an interpreter that uses normal-order evaluation?

Answer:

1. If it's applicate-order evaluation it will run infinitely. Because the
procedure with name (p) will recursively apply to itself (trying to evaluate
itself). So the expression `(test 0 (p))` never exists, because the sub
expression `(p)` never returns.

2. If it's normal-order evaluation, it will return the `consequent` value of
the if special form, because the argument `x` will be substituted with `0`
and the sub expression `(= x 0)` yields true, which  results in returning the
consequent value `0`

### 1.6

It's because of the evaluation strategy. `if` is a special form, so the
consequent and alternative are not evaluated until the predicate is known.
However `new-if` is a new procedure, and because Scheme uses applicate-order
evaluation, all arguments are evaluated. What this means it, it infinitely
evaluates `sqrt-iter`, which evaluates itself recursively until its out of
memory.

### 1.7

Two example of small and large numbers that makes it fails are:

```
(sqrt 0.000016)
(sqrt 81234567890123)
```

The small number is failing because the square root of `0.000016` is 0.004` and
the tolerance of `0.001` itself is enough to skew the guess, because it has
little difference with the original value.

The large number is failing because it takes a very long time to get the right
gueest to the high number of retrievals. Why? Because substraction `0.001` from
a very large number doesn't affect the result anymore due to floating-point
numbers. (think of substracting `0.0001` from infinity, nothing changes right). 

A better `good-enough?` precade would be (procedures that are changed from the
previous implementation):

```lisp
(define (sqrt-iter guess prev-guess x)
  (if (good-enough? guess prev-guess)
      guess
      (sqrt-iter (improve guess x) guess x)))

;; we can also define 0.001 as (/ guess 1000) to represent it as a fraction of
;; the guess instead of an hardcoded value
(define (good-enough? guess prev-guess)
  (< (abs (- guess prev-guess)) 0.001))

(define (sqrt x) (sqrt-iter 1.0 0.0 x))
```

This yields better results for both small and large numbers. For small numbers
it yield better results because we are not bound to the actuall result and can
iterate more precisely. For large numbers it yield better because the number of
iterations are lower than before and we are again bound to the tolerance of
`0.001`


## 1.8

All definitions with examples can be seen follow. This is exactly the same as
with `sqrt` procedure, we only changed the `improve` procedure with the
procedure given in the book. The follow definitions also includes the guess
improvement from the following example (1.7)

```lisp
(define (cbrt-iter guess prev-guess x)
  (if (good-enough? guess prev-guess)
      guess
      (cbrt-iter (improve x guess) guess x)))

(define (improve x guess)
  (/ (+ (/ x (square guess)) (* 2 guess)) 3))

(define (good-enough? guess prev-guess)
  (< (abs (- guess prev-guess)) (/ guess 1000)))

(define (square x) (* x x))
(define (abs x)
  (cond ((< x 0) (- x))
        ((> x 0) x)
        ((= x 0) 0)))

(define (cbrt x) (cbrt-iter 1.0 0.0 x))

;; examples
(cbrt 0.001)
(cbrt 27)
(cbrt 64)
(cbrt 125)
```

## 1.9

```lisp
(define (+ a b)
  (if (= a 0) b (inc (+ (dec a) b))))
```

(I've forgot to depict the `dec` expressions)

```
(+ 4 5)
(inc (+ 3 5))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc (5)))))
(inc (inc (inc (6))))
(inc (inc (7)))
(inc (8))
9
```

```lisp
(define (+ a b)
  (if (= a 0) b (+ (dec a) (inc b))))
```

```
(+ 4 5) 
(+ (dec 4) (inc 5)) 
(+ 3 6) 
(+ (dec 3) (inc 6)) 
(+ 2 7) 
(+ (dec 2) (inc 7)) 
(+ 1 8) 
(+ (dec 1) (inc 8)) 
(+ 0 9) 
9
```

As seen above, the first process is recursive (it defers the operation of inc).
The second process is iterative as the state is known at any point.

## 1.10

I've didn't just plugged into the interpreter, instead tried to get the idea
behind it. Checkout the Wikipedia page for Ackermann function's, it's really
beautiful: https://en.wikipedia.org/wiki/Ackermann_function

Answers are:


```lisp
(A 1 10)
```

This definition expands into `(A (0) (A (0) (A (0) ...)))` up to 10 times. The
latest expression will be `(A 1 1)` which evaluates to 2. So this means we are
multiplying the value `2` up to 10 times. So it's basically `2 ^ 10` -> `1024`


```lisp
(A 2 4)
```

This definition evaluates into `(A 1 (A 1 4))` after a couple of iterations.
Because we know that `(A 1 N)` is equal to `2 ^ N` (from the previous
procedure), this simplifies to `(A 1 16)` (because (A 1 4) is equal to `2^4` ->
16). So basically  we end up with `(A 1 16)`, which is `2 ^ 16` -> `65536`


```lisp
(A 3 3)
```

This definition evalutes into `(A 2 (A 2 (A 3 1)))` after a couple of
iterations. Which is then evaluated as `(A 2 (A 2 2))`. Evaluating `(A 2 2)`
gives us 4, so the final form is `(A 2 4)` which is the same as the previous
example. So the result is `2 ^ 16` -> `65536`


Define the following procedures as mathematical definitions:


```lisp
(define (f n) (A 0 n)) 
```
This is very easy because it always calls the expression `2*y`, so this is mathetmically: `2n`


```lisp
(define (g n) (A 1 n)) 
```
This is also known (from the previous definitions), we can define it as: `2^n`


```lisp
(define (h n) (A 2 n)) 
```

We have already two solutions from the above, adding the rest we got

```
(A 2 1) -> 2
(A 2 2) -> 4
(A 2 3) -> 16
(A 2 4) ->  65536
```

Looking carefuly we can see this all about the power of two. So it's basically: `2^2^2...` n times

All the definitions above are invalid of `n < 0` and `0` for the case `n = 0`
