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
collatz_cache_t collatz_cache(uint32_t limit) {
  collatz_cache_t cache;
  // we need some space to store previously calculated lengths. by using
  // calloc here we ensure that we don't need to manually memset the memory
  // to null, but it comes pre-zeroed.
  cache.limit = limit;
  cache.table = calloc(cache.limit, sizeof(uint32_t));
  if (!cache.table)
    return (collatz_cache_t){NULL, 0};

  return cache;
}
```

Allocate a new memoisation table capable of storing length of sequences with starting numbers up to `limit`.

### Get Length

```c
uint32_t collatz_length(uint32_t number, collatz_cache_t cache) {
  // when we reached 1, we know that we are at the end of the chain.
  if (number <= 1)
    return 1;

  // since 0 isn't a positive integer and we know that 1 is always the end
  // of the chain, we use the table to keep track of anything beyond one,
  // meaning that the indices are slightly shifted.
  size_t pos = number - 2;

  // first, look up our number in the table to see if we've encountered it
  // before. if so, return our cached result.
  if (pos < cache.limit && cache.table[pos] != 0)
    return cache.table[pos];

  // otherwise, find out which number is next in the chain
  uint32_t next;
  if ((number % 2) == 0) {
    next = number / 2;
  } else {
    next = 3 * number + 1;
  }

  // and recursively look up it's length, plus one for our current number.
  uint32_t length = 1 + collatz_length(next, cache);

  // save our result to the cache for future lookups
  if (pos < cache.limit) {
    cache.table[pos] = length;
  }

  return length;
}
```

Using the cache, compute what the collatz length of `number` is.

### Get Longest

```c
uint32_t collatz_longest(collatz_cache_t cache) {
  uint32_t max = 1;
  uint32_t index = 0;
  for (size_t i = 0; i < cache.limit; i++) {
    if (cache.table[i] > max) {
      max = cache.table[i];
      index = i;
    }
  }

  return index + 2;
}
```

From the cache, compute which number has the longest collatz sequence length.

### Release

```c
void collatz_free(collatz_cache_t *c) {
  free(c->table);
}
```

Release the memory used by the cache.
