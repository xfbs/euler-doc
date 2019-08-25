# Collatz

Modules to compute the length of collatz sequences up to a given number. A collatz sequence is defined to be a sequence obtained from taking a positive integer, and repeatedly applying the function

$$
C\colon\mathbb{N} \to \mathbb{N}, \quad C(n) = \begin{cases}
  n/2 & \text{if } n \text{ is even,}\\
  3 n + 1 & \text{if } n \text{ is odd.}
\end{cases}
$$

A collatz sequence is said to have ended once it reaches the value 1.

This module allows efficient computing of collatz sequence lengths by using a table to memoize them.

## Data Structures

## Reference

### Setup

```c
collatz_cache_t collatz_cache(uint32_t limit);
```

Allocate a new memoisation table capable of storing length of sequences with starting numbers up to `limit`.

### Get Length

```c
uint32_t collatz_length(uint32_t number, collatz_cache_t cache);
```

Using the cache, compute what the collatz length of `number` is.

### Get Longest

```c
uint32_t collatz_longest(collatz_cache_t cache);
```

From the cache, compute which number has the longest collatz sequence length.

### Release

```c
void collatz_free(collatz_cache_t *c);
```

Release the memory used by the cache.
