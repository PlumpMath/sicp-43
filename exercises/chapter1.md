## 1.1:

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


## 1.2:


```lisp
-> (/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5)))))
   (* 3 (- 6 2) (- 2 7)))
-37/150
```

## 1.3:

```lisp
(define (square a) (* a a))
(define (square-sum a b) (+ (square a) (square b)))
(define (largest-square-sum a b c)
  (cond ((< a b) (if (< a c) (square-sum b c) (square-sum a b)))
        (else (if (< b c) (square-sum a c) (square-sum a b)))))
```


## 1.4:

Explain:

```lisp
(define (a-plus-abs-b a b) 
  ((if (> b 0) + -) a b))
```

Desc:

We choose the operator based on the predicate value of the value `b` being
greater than 0 or not. If `b` is positive the operator result is `+` otherwise
it's `-`.  So our operator is the result of the compound expression, which is
then used for adding or subtracting to the operands `a` and `b`.

