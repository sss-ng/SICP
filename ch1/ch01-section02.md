## 1.2 - Procedures and the Processes They Generate

### Exercise 1.2.9

```scheme
(define (+ a b)
  (if (= a 0) b (inc (+ (dec a) b))))
```

`(+ 4 5)` produces a recursive process:

```
(+ 4 5)
(inc (+ 3 5))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc 5))))
(inc (inc (inc 6)))
(inc (inc 7))
(inc 8)
9
```

and for

```scheme
(define (+ a b)
  (if (= a 0) b (+ (dec a) (inc b))))
```

`(+ 4 5)` produces a linear process:

```
(+ 4 5)
(+ 3 6)
(+ 2 7)
(+ 1 8)
(+ 0 9)
9
```

### Exercise 1.2.10

```scheme
(define (A x y)
  (cond ((= y 0) 0)
    ((= x 0) (* 2 y))
    ((= y 1) 2)
    (else (A (- x 1) (A x (- y 1))))))

(define (f n) (A 0 n))
(define (g n) (A 1 n))
(define (h n) (A 2 n))
(define (k n) (* 5 n n))
```

`(f n)` produces $f(n) = 2n$
`(g n)` produces $g(n) = 2^n $
`(h n)` produces $h(n) = 2^{2^{2 (n-times)}}$

### Exercise 1.2.11

```scheme

#lang sicp

(define (f n)
  (if (< n 3)
      n
      (+
       (f (- n 1))
       (* 2 (f (- n 2)))
       (* 3 (f (- n 3))))))

(define (f2 n)
  (define (f-iter a b c count)
    (if (= count 2)
        a
        (f-iter
          (+ a (* 2 b) (* 3 c))
          a
          b
          (- count 1))))
  (if (< n 3)
      n
      (f-iter 2 1 0 n)))
```

### Exercise 1.2.12

```scheme
#lang sicp


(define (pascal row col)
  (if (or (<= col 1) (= row col) )
      1
      (+ (pascal (- row 1) col) (pascal (- row 1) (- col 1)))
  ))

(pascal 1 1)
(pascal 2 1)
(pascal 2 2)
(pascal 3 1)
(pascal 3 2)
(pascal 3 3)
(pascal 4 1)
(pascal 4 2)
(pascal 4 3)
(pascal 4 4)
```

### Exercise 1.2.13

To prove that $Fib(n)$ is the closest integer to $\phi ^ n / \sqrt{5}$, we will show that $Fib(n) = (\phi ^ n - \psi ^ n)/\sqrt{5}$. This works because $\psi$ is smaller than 1, so to a power it is also less than 1, which shows that Fib(n) is close-ish to $\phi ^ n / \sqrt{5}$.

We will prove this via induction.
Let $P(n)$ be the statement $Fib(n) = (\phi ^ n - \psi ^ n)/\sqrt{5}$.

$P(0)$ and $P(1)$ are obviously true by arithmetic.
Now lets assume that $P(k)$ is true for some $k$. We want to show $P(k+1)$ is also true.

Lets consider $P(k+1)$: $Fib(k+1) = (\phi ^ {k+1} - \psi ^ {k+1})/\sqrt{5}$

We know that

$Fib(k+1)$

$$
= Fib(k) + Fib(k-1)
=  (\phi ^ {k} - \psi ^ {k}) / \sqrt{5} + (\phi ^ {k-1} - \psi ^ {k-1}) / \sqrt{5} \\
=  ((\phi + 1) \phi ^ {k-1} - (\psi + 1) \psi ^ {k-1}) / \sqrt{5}\\
=  ((\phi ^ 2) \phi ^ {k-1} - (\psi ^ 2) \psi ^ {k-1}) / \sqrt{5}  \;\;\;\;(Note: 1)\\
=  (\phi ^ {k+1} - \psi ^ {k+1}) / \sqrt{5}
$$

$(Note: 1)$ The definition of $\phi$ and $\psi$ is that they are solutions to the equation $x^2 = x+1$.

$\square$

### Exercise 1.2.14

Not going to draw it in ascii, but did on my whiteboard. Basically, its a very skewed tree, where the max height is when you choose all pennies, so the space complexity is $O(n)$. Not really sure what the time complexity is, but given that there are 5 branches, I'd guess its exponential.

### Exercise 1.2.15

a) `(sine 12.15)` has `p` evaluated 5 times since you must divide 12.15 by 3, 5 times to get a number smaller than 0.1.

b) Growth in space and time is $O(\log_{3}n)$ since we have a linear recursive process, which uses up stack space since it isnt tail recursive.
