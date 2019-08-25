# Math

Module for various math functions.

## Reference

### Least Common Multiple

```c
uint64_t lcm(uint64_t a, uint64_t b);
```

Compute the least common multiple of two numbers, which is the smallest number
that is divisible by both numbers.

!!! example
    ```c
    assert(lcm(10, 5) == 10);
    assert(lcm(3, 5) == 15);
    assert(lcm(6, 14) == 42);
    ```

### Greatest Common Divisor

```c
uint64_t gcd(uint64_t a, uint64_t b);
```

Compute the greatest common divisor using euclid's algorithm, which is the
largest number that is a divisor of both numbers.

!!! example
    ```c
    assert(gcd(2, 3) == 1);
    assert(gcd(14, 12) == 2);
    assert(gcd(55, 11) == 11);
    ```

### Factorial

```c
uint64_t factorial(uint8_t nth);
```

Computes the nth factorial.

!!! example
    ```c
    assert(factorial(0) == 1);
    assert(factorial(1) == 1);
    assert(factorial(2) == 2);
    assert(factorial(3) == 6);
    assert(factorial(4) == 24);
    ```

### Divisors Sum

```c
uint32_t divisor_sum(uint32_t num);
```

Calculates the sum of proper divisors of num, meaning all divisors lower
than num itself.

!!! example
    ```c
    assert(divisor_sum(220) == 284);
    assert(divisor_sum(284) == 220);
    ```

### Palindrome

```c
bool is_palindrome(uint64_t num);
```

Checks if the given number is a palindrome.

!!! example
    ```c
    assert(is_palindrome(12321) == true);
    assert(is_palindrome(445644) == false);
    ```

### Pandigital

```c
uint32_t nth_pandigital(uint8_t n, uint32_t nth);
```

Return the `nth` pandigital of length $n$.

A pandigital is a number making use of all numbers from 0 to $n$ exactly
once, therefore it is a permutation of the set $[0..n]$.

!!! todo
    - [ ] Write this as a generic permutation function.
    - [ ] Add code example.

### Fibonacci

```c
uint64_t fibonacci(uint64_t nth);
```

Computes the `nth` fibonacci number.

Fibonacci numbers are a sequence that starts with 1, 1 and each successive
number is the sum of the previous two numbers. Since computing it like that
is very expensive, this function computes the `nth` fibonacci number in an
efficient manner using `PHI` and `ROOT5`.

!!! example
    ```c
    assert(fibonacci(0) == 1);
    assert(fibonacci(1) == 1);
    assert(fibonacci(2) == 2);
    assert(fibonacci(3) == 3);
    assert(fibonacci(4) == 5);
    assert(fibonacci(5) == 8);
    ```

