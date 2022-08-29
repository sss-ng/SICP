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

$$Fib(k+1) $$

$$= Fib(k) + Fib(k-1)$$

$$= (\phi ^ {k} - \psi ^ {k}) / \sqrt{5} + (\phi ^ {k-1} - \psi ^ {k-1}) / \sqrt{5}$$

$$= ((\phi + 1) \phi ^ {k-1} - (\psi + 1) \psi ^ {k-1}) / \sqrt{5}$$

$$= ((\phi ^ 2) \phi ^ {k-1} - (\psi ^ 2) \psi ^ {k-1}) / \sqrt{5}    \text{ (See Note: 1)}$$

$$= (\phi ^ {k+1} - \psi ^ {k+1}) / \sqrt{5}$$

$(Note: 1)$ The definition of $\phi$ and $\psi$ is that they are solutions to the equation $x^2 = x+1$.

$\square$

### Exercise 1.2.14

Not going to draw it in ascii, but did on my whiteboard. Basically, its a very skewed tree, where the max height is when you choose all pennies, so the space complexity is $O(n)$. Not really sure what the time complexity is, but given that there are 5 branches, I'd guess its exponential.

### Exercise 1.2.15

a) `(sine 12.15)` has `p` evaluated 5 times since you must divide 12.15 by 3, 5 times to get a number smaller than 0.1.

b) Growth in space and time is $O(\log_{3}n)$ since we have a linear recursive process, which uses up stack space since it isnt tail recursive.

### Exercise 1.2.16

```scheme
#lang sicp

(define (even? n)
  (= (remainder n 2) 0))

(define (square n) (* n n))

(define (fast-expt-iter b n a)
  (cond ((= n 0) a)
        ((even? n) (fast-expt-iter (square b) (/ n 2) a))
        (else (fast-expt-iter b (- n 1) (* a b)))))

;; (fast-expt-iter 2 10 1)
;; 1048
```

### Exercise 1.2.17

```scheme
#lang sicp

(define (even? n)
  (= (remainder n 2) 0))

(define (double n) (+ n n))

(define (halve n) (/ n 2))

(define (fast-mult a b)
  (cond ((= a 0) 0)
        ((even? a) (double (fast-mult (halve a) b)))
        (else (+ b (fast-mult (- a 1) b)))))


;; (fast-mult 10 5)
;; (fast-mult 4 8)
;; 50
;; 32
```

### Exercise 1.2.18

```scheme
(define (even? n)
  (= (remainder n 2) 0))

(define (double n) (+ n n))

(define (halve n) (/ n 2))

(define (fast-mult-iter a b s)
  (cond ((= a 0) s)
        ((even? a) (fast-mult-iter (halve a) (double b) s))
        (else (fast-mult-iter (- a 1) b (+ s b)))))
;; (fast-mult-iter 10 5)
;; (fast-mult-iter 4 8)
;; 50
;; 32
```

### Exercise 1.2.19

After a ton of algebra that Im really not interested in writing out, you get that:
$q' = 2pq + q^2$ and $p' = p^2 + q^2$.

```scheme
#lang sicp

(define (even? n)
  (= (remainder n 2) 0))

(define (square n) (* n n))

(define (fib n)
  (fib-iter 1 0 0 1 n))

(define (fib-iter a b p q count)
  (cond ((= count 0) b)
    ((even? count)
    (fib-iter a
              b
              (+ (square p) (square q)) ; compute p′
              (+ (* 2 p q) (square q)); compute q′
              (/ count 2)))
    (else (fib-iter (+ (* b q) (* a q) (* a p))
                    (+ (* b p) (* a q))
                    p
                    q
                    (- count 1)))))

;; (fib 5)
;; (fib 6)
;; (fib 7)
;; (fib 8)

;; 5
;; 8
;; 13
;; 21

```

### Exercise 1.2.20

The idea behind normal order evaluation is that you expand everything and then evaluate later.

Applicative order evaluation will proceed like so:

```scheme
(gcd 206 40)
(gcd 206 (remainder 206 40))
(gcd 40 (remainder 40 6))
(gcd 6 (remainder 6 4))
(gcd 4 (remainder 4 2))
(gcd 2 0)
;; 2
```

So, `remainder` is called 4 times.

There's probably a typo somewhere in the below but I think the count is correct:

```scheme

(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))

;; (gcd 206 40)


(if (= 40 0)
    206
    (gcd 40 (remainder 206 40)))


(if (= (remainder 206 40) 0) ;; 1
    40
    (gcd (remainder 206 40) (remainder 40 (remainder 206 40))))


(if (= (remainder 40 (remainder 206 40)) 0) ;; 2
    (remainder 206 40)
    (gcd
     (remainder 40 (remainder 206 40))
     (remainder (remainder 206 40)
                (remainder 40
                           (remainder 206 40)))))

(if (= (remainder (remainder 206 40)
                (remainder 40
                           (remainder 206 40)))
       0) ;; 4
    (remainder 40 (remainder 206 40))
    (gcd
     (remainder (remainder 206 40)
                (remainder 40
                           (remainder 206 40)))
     (remainder
      (remainder 40 (remainder 206 40))
      (remainder (remainder 206 40)
                 (remainder 40
                           (remainder 206 40))))))

(if (= (remainder
      (remainder 40 (remainder 206 40))
      (remainder (remainder 206 40)
                 (remainder 40
                           (remainder 206 40))))
       0) ;; 7
    (remainder (remainder 206 40)
                (remainder 40
                           (remainder 206 40))) ;; 4
    (gcd
     (remainder
      (remainder 40 (remainder 206 40))
      (remainder (remainder 206 40)
                 (remainder 40
                           (remainder 206 40))))
     (remainder
      (remainder (remainder 206 40)
                (remainder 40
                           (remainder 206 40)))
      (remainder
       (remainder 40 (remainder 206 40))
       (remainder (remainder 206 40)
                  (remainder 40
                             (remainder 206 40)))))))
```

So the count is $1+2+4+7+4=18$.

### Exercise 1.2.21

```scheme
#lang sicp

(define (smallest-divisor n) (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b) (= (remainder b a) 0))

(define (square n) (* n n))

(smallest-divisor 199)
(smallest-divisor 1999)
(smallest-divisor 19999)
;; 199
;; 1999
;; 7
```

### Exercise 1.2.22

```scheme

(define (smallest-divisor n) (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b) (= (remainder b a) 0))

(define (square n) (* n n))

(define (prime? n)
  (= n (smallest-divisor n)))

(define (timed-prime-test n)
  (newline)
  (display n)
  (start-prime-test n (runtime)))

(define (start-prime-test n start-time)
  (if (prime? n)
      (report-prime (- (runtime) start-time))))

(define (report-prime elapsed-time)
  (display " *** ")
  (display elapsed-time))

(define (search-for-primes start end)
  (cond ((even? start)
         (search-for-primes (+ 1 start) end))
        ((<= start end)
         (begin
           (timed-prime-test start)
           (search-for-primes (+ 2 start) end)))))

(search-for-primes 1000 1000000)
```

The times for each run dont really register on modern hardware...

### Exercise 1.2.23

```scheme
#lang sicp

(define (smallest-divisor n) (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (next test-divisor)))))

(define (divides? a b) (= (remainder b a) 0))

(define (square n) (* n n))

(define (next n)
  (if (= n 2) 3
      (+ n 2)))

```

Not sure why it would be halved. Maybe there is less numerical overhead when calling `divides?` on even numbers, or maybe the extra `if` check in `next`.

### Exercise 1.2.24

```scheme
#lang sicp

(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder (square (expmod base (/ exp 2) m))
                    m))
        (else
         (remainder
          (* base (expmod base (- exp 1) m))
          m))))

(define (smallest-divisor n) (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (next 1)))))

(define (next n)
  (if (= n 2) 3
      (+ n 2)))

(define (divides? a b) (= (remainder b a) 0))

(define (square n) (* n n))

(define (prime? n)
  (= n (smallest-divisor n)))

(define (timed-prime-test n)
  (newline)
  (display n)
  (start-prime-test n (runtime)))

(define (start-prime-test n start-time)
  (if (fast-prime? n 100)
      (report-prime (- (runtime) start-time))))

(define (report-prime elapsed-time)
  (display " *** ")
  (display elapsed-time))

(define (fermat-test n)
  (define (try-it a)
    (= (expmod a n n) a))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((fermat-test n) (fast-prime? n (- times 1)))
        (else false)))

```

### Exercise 1.2.25

The numbers will likely get huge. In theory thats ok but most likely the machine wont be able to handle them as efficiciently.

### Exercise 1.2.26

Louis Reasoner's method basically forces normal order evaluation onto the process. `(square n)` only evaluates `n` once, while their squaring process evaluates it twice. So now the process is tree recursive, but because its also logarithmic, they cancel each other out and become just a linear `O(n)` process.

### Exercise 1.2.27

```scheme
(define (carmichael n a)
  (cond ((= a n) #t)
        ((or (> a n) (not (= (expmod a n n) a))) #f)
        (else (carmichael n (+ a 1)))))

(carmichael 6601 1)
;; #t
```

### Exercise 1.2.28

```scheme
#lang sicp

(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder (square-special (expmod base (/ exp 2) m))
                    m))
        (else
         (remainder
          (* base (expmod base (- exp 1) m))
          m))))

(define (square-special n m)
  (cond ((and (= (remainder (square n) m) 1)
              (not (= n (- m 1)))
              (not (= n 1)))
         0)
        (else (square n))))

(define (miller-rabin-helper a n)
  (cond ((= a 0) #t)
        ((= (expmod a (- n 1) n) 1) (miller-rabin (- a 1) n))
        (else #f)))

(define (miller-rabin n)
  (miller-rabin-helper (- n 1) n))
```
