# Math

Module for various math functions.

## Constants

### Phi

```c
static const double PHI = 1.61803398875;
```

Phi.

### Root of 5.

```c
static const double ROOT5 = 2.236067977;
```

This is the precomputed root of 5.

## Reference

### Least Common Multiple

```c
uint64_t lcm(uint64_t a, uint64_t b) {
  if (b > a)
    return lcm(b, a);
  return a / gcd(a, b) * b;
}
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
uint64_t gcd(uint64_t a, uint64_t b) {
  while (b != 0) {
    uint64_t t = b;
    b = a % b;
    a = t;
  }

  return a;
}
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
uint64_t factorial(uint8_t nth) {
  static uint64_t cache[19] = {0};
  if (nth <= 1)
    return 1;

  if (nth <= 20) {
    if (!cache[nth - 2])
      cache[nth - 2] = factorial(nth - 1) * nth;

    return cache[nth - 2];
  } else {
    // 21! can't be represented in a uint64_t, this is an overflow.
    return 0;
  }
}
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
uint32_t divisor_sum(uint32_t num) {
  uint32_t sum = 0;

  for (uint32_t i = 1; i <= (uint32_t)sqrt(num); i++) {
    if ((num % i) == 0) {
      sum += i;
      if (i != (num / i) && i != 1) {
        sum += (num / i);
      }
    }
  }

  return sum;
}
```

Calculates the sum of proper divisors of num, meaning all divisors lower
than num itself. This could maybe be improved by only checking for prime
number divisors, and adding up the product of any combinations of them.

!!! example
    ```c
    assert(divisor_sum(220) == 284);
    assert(divisor_sum(284) == 220);
    ```

### Palindrome

```c
bool is_palindrome(uint64_t num) {
  // reverse the number and compare against itself
  uint64_t original = num;
  uint64_t reversed = 0;

  while (num != 0) {
    reversed *= 10;
    reversed += num % 10;
    num /= 10;
  }

  return original == reversed;
}
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
uint64_t fibonacci(uint64_t nth) {
  return round(pow(PHI, nth + 1) / ROOT5);
}
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

