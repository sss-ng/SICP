## 1.1 - The Elements of Programming

### Exercise 1.1.1

```scheme
> 10
10
> (+ 5 3 4)
12
> (- 9 1)
8
> (/ 6 2)
3
> (+ (* 2 4) (- 4 6))
6
> (define a 3)
> (define b (+ a 1))
> b
4
> (+ a b (* a b))
19
> (= a b)
#f
> (if (and (> b a) (< b (* a b)))
      b
      a)
4
> (cond ((= a 4) 6)
        ((= b 4) (+ 6 7 a))
        (else 25))
16
```

### Exercise 1.1.2

```scheme
#lang sicp

(/
 (+ 5 4 (- 2
           (- 3
              (+ 6
                 (/ 4 5)))))
 (* 3
    (- 6 2)
    (- 2 7)))
```

### Exercise 1.1.3

```scheme
#lang sicp

(define (sum-square-larger a b c)
  (cond ((< a b)
          (if (< a c)
             (+ (* b b) (* c c))   ;; a is smallest
             (+ (* b b) (* a a)))) ;; c is smallest
        (else
          (if (< b c)
             (+ (* a a) (* c c))   ;; b is smallest
             (+ (* b b) (* a a)))) ;; c is smallest
        ))

(sum-square-larger 1 2 3)
;; 13
(sum-square-larger 1 3 2)
;; 13
(sum-square-larger 2 1 3)
;; 13
(sum-square-larger 2 3 1)
;; 13
(sum-square-larger 3 1 2)
;; 13
(sum-square-larger 3 2 1)
;; 13
```

A slightly cleaner version assuming we have `<=`:

```scheme
#lang sicp

(define (square x)
  (* x x))

(define (sum-square a b)
  (+ (square a) (square b)))

(define (sum-square-larger a b c)
  (cond ((and (<= a b) (<= a c)) ;; a is the min of a, b, c
         (sum-square b c))
        ((and (<= b a) (<= b c)) ;; b is the min of a, b, c
         (sum-square a c))
        (else                    ;; c is the min of a, b, c
         (sum-square a b))))
```

### Exercise 1.1.4

```scheme
(define (a-plus-abs-b a b)
  ((if (> b 0) + -) a b))
```

The above returns `+` or `-` depending if `b` is positive or negative. Then this primitive is applied to `a` and `b`.

### Exercise 1.1.5

```scheme
(define (p) (p))

(define (test x y)
  (if (= x 0)
      0
      y))

(test 0 (p))
```

With `applicative-order` evaluation, Ben will get an infinite loop that will eventually cause the process to error. This is because the parameters of the procedure call will be evaluated first, leading to the execution of the recursive procedure `p` without a base case.

When using `normal-order` evaluation, a value will be returned: `0`. This is because normal-order evaluation will 'fully expand, then reduce' parameters in a procedure. Therefore, the ill conditioned procedure `p` is never called, since the control flow of the procedure bypasses it entirely.

### Exercise 1.1.6

`Scheme` uses applicative order evaluation. That is, a procedure's parameters are evaluated before the actual procedure is. So, in this case, rewriting `if` as a procedure will cause the process to recurse infinitely - which will cause it to crash.

### Exercise 1.1.7

```scheme
# lang sicp

(define (sqrt-iter guess prev-guess x)
  (if (good-enough? guess prev-guess)
      guess
      (sqrt-iter (improve guess x) guess x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess prev-guess)
  (< (/ (abs (- prev-guess guess)) prev-guess) 0.001))

(define (sqrt x)
  (sqrt-iter 1.0 2.0 x))
```

Example of error:

```scheme
> (sqrt 0.0000001)
0.03125106561775382
```

We should get something more like 0.000316...
With the new code above, comparing `guess` to `prev-guess`, we do.

### Exercise 1.1.8

Newtons method for cube roots

```scheme
# lang sicp

(define (cube-rt-iter guess prev-guess x)
  (if (good-enough? guess prev-guess)
      guess
      (cube-rt-iter (improve guess x) guess x)))

(define (improve guess x)
  (/
    (+
      (/ x (* guess guess))
      (* 2 guess))
    3))

(define (good-enough? guess prev-guess)
  (< (/ (abs (- prev-guess guess)) prev-guess) 0.001))

(define (cube-rt x)
  (cube-rt-iter 1.0 2.0 x))
```
