# Algebra

Table of Contents

- [Fundamentals](#Fundamentals)
 - [Binary Exponentiation](#Binary-Exponentiation)
 - [Euclidean algorithm for computing the greatest common divisor](#Euclidean-algorithm-for-computing-the-greatest-common-divisor)
 - [Fibonacci Numbers](#Fibonacci-Numbers)

## Fundamentals

### [Binary Exponentiation](https://cp-algorithms.com/algebra/binary-exp.html)

Binary exponentiation (also known as exponentiation by squaring) is a trick which allows to calculate $a^{n}$ using only O(logn) multiplications (instead of O(n) multiplications required by the naive approach). 

Raising a to the power of n is expressed naively as multiplication by a done n−1 times: an=a⋅a⋅…⋅a. However, this approach is not practical for large a or n.

The idea of binary exponentiation is, that we split the work using the binary representation of the exponent.

Let's write n in base 2, for example: $3^{13}$ = $3^{1101_{2}}$ = $3^{8}$.$3^{4}$$3^{1}$ 
Since the number n has exactly ⌊log2n⌋+1 digits in base 2, we only need to perform O(logn) multiplications, if we know the powers $a^{1}$, $a^{2}$, $a^{4}$, $a^{8}$,..., $a^{logn}$

The final complexity of this algorithm is O(logn): we have to compute logn powers of a, and then have to do at most logn multiplications to get the final answer from them.

So we only need to know a fast way to compute those. Luckily this is very easy, since an element in the sequence is just the square of the previous element.

$\begin{align}
3^1 &= 3 \\
3^2 &= \left(3^1\right)^2 = 3^2 = 9 \\
3^4 &= \left(3^2\right)^2 = 9^2 = 81 \\
3^8 &= \left(3^4\right)^2 = 81^2 = 6561
\end{align}$

So to get the final answer for $3^{13}$, we only need to multiply three of them (skipping 32 because the corresponding bit in n is not set):  $3^{13}$=6561⋅81⋅3=1594323

The following recursive approach expresses the same idea:

$a^n = \begin{cases}
1 &\text{if } n == 0 \\
\left(a^{\frac{n}{2}}\right)^2 &\text{if } n > 0 \text{ and } n \text{ even}\\
\left(a^{\frac{n - 1}{2}}\right)^2 \cdot a &\text{if } n > 0 \text{ and } n \text{ odd}\\
\end{cases}$

Recursive Approach:


```python
def beRecursive(a, b):
    if b == 0:
        return 1
    
    res = beRecursive(a, b // 2)
    
    if b % 2 == 0:
        return res * res
    else:
        return res * res * a
    
beRecursive(3, 13)
```




    1594323



The second approach accomplishes the same task without recursion. It computes all the powers in a loop, and multiplies the ones with the corresponding set bit in n. Although the complexity of both approaches is identical, this approach will be faster in practice since we have the overhead of the recursive calls.


```python
def beIterative(a, b):
    res = 1
    
    while b > 0:
        if b & 1:
            res = res * a
        a = a * a
        b >>= 1
    
    return res

beIterative(3, 13)
```




    1594323



#### Variation of binary exponentiation: multiplying two numbers modulo m

Problem: Multiply two numbers a and b modulo m. a and b fit in the built-in data types, but their product is too big to fit in a 64-bit integer. The idea is to compute a⋅b(modm) without using bignum arithmetics.

Solution: We simply apply the binary construction algorithm described above, only performing additions instead of multiplications. In other words, we have "expanded" the multiplication of two numbers to O(logm) operations of addition and multiplication by two (which, in essence, is an addition).

$a \cdot b = \begin{cases}
0 &\text{if }a = 0 \\
2 \cdot \frac{a}{2} \cdot b &\text{if }a > 0 \text{ and }a \text{ even} \\
2 \cdot \frac{a-1}{2} \cdot b + b &\text{if }a > 0 \text{ and }a \text{ odd}
\end{cases}$

### [Euclidean algorithm for computing the greatest common divisor](https://cp-algorithms.com/algebra/euclid-algorithm.html)

Given two non-negative integers a and b, we have to find their GCD (greatest common divisor), i.e. the largest number which is a divisor of both a and b. It's commonly denoted by gcd(a,b). Mathematically it is defined as: $\gcd(a, b) = \max_ {k = 1 \dots \infty ~ : ~ k \mid a ~ \wedge k ~ \mid b} k.$

(here the symbol "∣" denotes divisibility, i.e. "k∣a" means "k divides a")

When one of the numbers is zero, while the other is non-zero, their greatest common divisor, by definition, is the second number. When both numbers are zero, their greatest common divisor is undefined (it can be any arbitrarily large number), but we can define it to be zero. Which gives us a simple rule: if one of the numbers is zero, the greatest common divisor is the other number.

The Euclidean algorithm, discussed below, allows to find the greatest common divisor of two numbers a and b in O(logmin(a,b)).

Algorithm

The algorithm is extremely simple:
$\gcd(a, b) = \begin{cases}a,&\text{if }b = 0 \\ \gcd(b, a \bmod b),&\text{otherwise.}\end{cases}$

Implementation:


```python
def gcdRecursive(a, b):
    if b == 0:
        return a
    return gcdRecursive(b, a % b)

def gcdIterative(a, b):
    while b != 0:
        a %= b
        a, b = b, a
    return a

print('Recursive: ', gcdRecursive(10, 5))
print('Iterative: ', gcdIterative(10, 5))
```

    Recursive:  5
    Iterative:  5


#### Least common multiple

Calculating the least common multiple (commonly denoted LCM) can be reduced to calculating the GCD with the following simple formula:

$\text{lcm}(a, b) = \frac{a \cdot b}{\gcd(a, b)}$

Thus, LCM can be calculated using the Euclidean algorithm with the same time complexity:

Implementation:


```python
def lcm(a, b):
    return (a * b) // gcdRecursive(a, b)

lcm(10, 2)
```




    10



### [Fibonacci Numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html)

he Fibonacci sequence is defined as follows: $F_0 = 0, F_1 = 1, F_n = F_{n-1} + F_{n-2}$

The first elements of the sequence are: 0,1,1,2,3,5,8,13,21,34,55,89,...

#### Properties

Fibonacci numbers possess a lot of interesting properties. Here are a few of them:

- Cassini's identity:  $F_{n-1} F_{n+1} - F_n^2 = (-1)^n$

- The "addition" rule: $F_{n+k} = F_k F_{n+1} + F_{k-1} F_n$

- Applying the previous identity to the case k=n, we get: $F_{2n} = F_n (F_{n+1} + F_{n-1})$

- From this we can prove by induction that for any positive integer k, $F_{nk}$ is multiple of $F_n$.

- The inverse is also true: if $F_m$ is multiple of $F_n$, then m is multiple of n.

- GCD identity: $GCD(F_m, F_n) = F_{GCD(m, n)}$

- Fibonacci numbers are the worst possible inputs for Euclidean algorithm


#### Formulas for the n-th Fibonacci number

The n-th Fibonacci number can be easily found in O(n) by computing the numbers one by one up to n. However, there are also faster ways, as we will see.

- ##### Closed-form expression

There is a formula known as "Binet's formula", even though it was already known by Moivre: 

$F_n = \frac{\left(\frac{1 + \sqrt{5}}{2}\right)^n - \left(\frac{1 - \sqrt{5}}{2}\right)^n}{\sqrt{5}}$

You can immediately notice that the second term's absolute value is always less than 1, and it also decreases very rapidly (exponentially). Hence the value of the first term alone is "almost" Fn. This can be written strictly as:

$F_n = \left[\frac{\left(\frac{1 + \sqrt{5}}{2}\right)^n}{\sqrt{5}}\right]$

where the square brackets denote rounding to the nearest integer.

As these two formulas would require very high accuracy when working with fractional numbers, they are of little use in practical calculations.

- ##### Matrix Form

It is easy to prove the following relation:

$\begin{pmatrix}F_{n-1} & F_{n} \cr\end{pmatrix} = \begin{pmatrix}F_{n-2} & F_{n-1} \cr\end{pmatrix} \cdot \begin{pmatrix}0 & 1 \cr 1 & 1 \cr\end{pmatrix}$

Denoting $P \equiv \begin{pmatrix}0 & 1 \cr 1 & 1 \cr\end{pmatrix}$, we have

$\begin{pmatrix}F_n & F_{n+1} \cr\end{pmatrix} = \begin{pmatrix}F_0 & F_1 \cr\end{pmatrix} \cdot P^n$

Thus, in order to find $F_n$, we must raise the matrix P to n. This can be done in O(logn)

## Prime Numbers

### [Sieve of Eratosthenes](https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html)

Sieve of Eratosthenes is an algorithm for finding all the prime numbers in a segment [1;n] using O(nloglogn) operations.

The algorithm is very simple: at the beginning we write down all numbers between 2 and n. We mark all proper multiples of 2 (since 2 is the smallest prime number) as composite. A proper multiple of a number x, is a number greater than x and divisible by x. Then we find the next number that hasn't been marked as composite, in this case it is 3. Which means 3 is prime, and we mark all proper multiples of 3 as composite. The next unmarked number is 5, which is the next prime number, and we mark all proper multiples of it. And we continue this procedure until we processed all numbers in the row.

<img src="https://raw.githubusercontent.com/e-maxx-eng/e-maxx-eng/master/img/sieve_eratosthenes.png">

The idea behind is this: A number is prime, if none of the smaller prime numbers divides it. Since we iterate over the prime numbers in order, we already marked all numbers, who are divisible by at least one of the prime numbers, as divisible. Hence if we reach a cell and it is not marked, then it isn't divisible by any smaller prime number and therefore has to be prime.

Implementation:


```python
def sieveOfEratosthenes(n):
    is_prime = [True for _ in range(n + 1)]
    is_prime[0] = is_prime[1] = False
    
    for i in range (2, n+1):
        if is_prime[i] and i*i <= n:
            for j in range(i*i, n+1, i):
                is_prime[j] = False
    
    return is_prime

sieveOfEratosthenes(10)
```




    [False, False, True, True, False, True, False, True, False, False, False]



This code first marks all numbers except zero and one as potential prime numbers, then it begins the process of sifting composite numbers. For this it iterates over all numbers from 2 to n. If the current number i is a prime number, it marks all numbers that are multiples of i as composite numbers, starting from i2. This is already an optimization over naive way of implementing it, and is allowed as all smaller numbers that are multiples of i necessary also have a prime factor which is less than i, so all of them were already sifted earlier. Since i2 can easily overflow the type int, the additional verification is done using type long long before the second nested loop.

Using such implementation the algorithm consumes O(n) of the memory (obviously) and performs O(nloglogn)

The biggest weakness of the algorithm is, that it "walks" along the memory multiple times, only manipulating single elements. This is not very cache friendly. And because of that, the constant which is concealed in O(nloglogn) is comparably big.

Besides, the consumed memory is a bottleneck for big n.

#### Sieving till root

Obviously, to find all the prime numbers until n, it will be enough just to perform the sifting only by the prime numbers, which do not exceed the root of n.


### Sieve of Eratosthenes Having Linear Time Complexity

The standard way of solving a task is to use the sieve of Eratosthenes. This algorithm is very simple, but it has runtime O(nloglogn).

Besides, the algorithm given here calculates factorizations of all numbers in the segment [2;n] as a side effect, and that can be helpful in many practical applications.

The weakness of the given algorithm is in using more memory than the classic sieve of Eratosthenes': it requires an array of n numbers, while for the classic sieve of Eratosthenes it is enough to have n bits of memory (which is 32 times less). Thus, it makes sense to use the described algorithm only until for numbers of order 107 and not greater.

#### Algorithm

Our goal is to calculate minimum prime factor lp[i] for every number i in the segment [2;n].

Besides, we need to store the list of all the found prime numbers - let's call it pr[].

We'll initialize the values lp[i] with zeros, which means that we assume all numbers are prime. During the algorithm execution this array will be filled gradually.

Now we'll go through the numbers from 2 to n. We have two cases for the current number i:

- lp[i]=0  - that means that i is prime, i.e. we haven't found any smaller factors for it.
Hence, we assign lp[i]=i and add i to the end of the list pr[].

- lp[i]≠0 - that means that i is composite, and its minimum prime factor is lp[i].

In both cases we update values of lp[] for the numbers that are divisible by i. However, our goal is to learn to do so as to set a value lp[] at most once for every number. We can do it as follows:

Let's consider numbers xj=i⋅pj, where pj are all prime numbers less than or equal to lp[i] (this is why we need to store the list of all prime numbers).

We'll set a new value lp[xj]=pj for all numbers of this form.

Implementation:


```python
def sieveOfEratosthenes(n):
    lp = [0 for _ in range(n+1)]
    pr = []
    
    for i in range(2, n+1):
        if lp[i] == 0:
            lp[i] = i
            pr.append(i)
        
        for j in range(len(pr)):
            if pr[j] <= lp[i] and i*pr[j] <= n:
                break
            
            l[i*pr[j]] = pr[j]
```

### 
