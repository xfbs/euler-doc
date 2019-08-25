# Modular Arithmetic

Module for performing modular arithmetic. 

## Reference

### Find Last Set Bit

```c
static uint8_t flsll(uint64_t mask) {
  if (mask == 0)
    return 0;

  // find last set bit
  uint8_t bit;
  for (bit = 1; mask != 1; bit++) {
    mask = mask >> 1;
  }

  return bit;
}
```

Given a number, find the index of the highest set bit.

### Exponentiation

```c
uint64_t pow_mod(uint64_t num, uint64_t exp, uint64_t mod) {
  if (exp == 1) {
    // num^1 % mod == num % mod
    return num % mod;
  } else {
    // this is (num*num)^(floor(exp/2)) % mod
    uint64_t res = pow_mod(mult_mod(num, num, mod), exp / 2, mod);

    if ((exp % 2) == 1) {
      // num^(2e+1) % mod == num*(num*num)^e % mod
      return mult_mod(res, num, mod);
    } else {
      // num^(2e) % mod == (num*num)^e % mod
      return res % mod;
    }
  }
}
```

Perform exponentiation. 

$$
\text{pow\_mod}(n, e, m) = n^e \mod m
$$

### Multiplication

```c
uint64_t mult_mod(uint64_t a, uint64_t b, uint64_t mod) {
  // if numbers are small enough, just
  // multiply them now
  if ((flsll(a) + flsll(b)) < 64) {
    return (a * b) % mod;
  }

  // extract high and low parts of numbers
  uint64_t ah = a >> 32;
  uint64_t bh = b >> 32;
  uint64_t al = a & (((uint64_t)1 << 32) - 1);
  uint64_t bl = b & (((uint64_t)1 << 32) - 1);

  uint64_t res = 0;
  res += (al * bl) % mod;
  res += shift_mod((al * bh) % mod, 32, mod);
  res %= mod;
  res += shift_mod((ah * bl) % mod, 32, mod);
  res %= mod;
  res += shift_mod((ah * bh) % mod, 64, mod);
  res %= mod;

  return res;
}
```

Perform modular multiplication of two 64-bit numbers. Works by splitting the two numbers into four 32-bit numbers, multiplying those separately, and then adding them up.

### Shift

```c
uint64_t shift_mod(uint64_t num, uint8_t amount, uint64_t mod) {
  num %= mod;

  uint8_t num_size = flsll(num);
  if (num_size + amount < 64) {
    return (num << amount) % mod;
  } else {
    uint8_t space = 64 - num_size;
    assert(space != 0);

    return shift_mod(num << space, amount - space, mod);
  }
}
```

Perform left shift.

### Addition

```c
uint64_t sum_mod(uint64_t num[], size_t len, uint64_t mod) {
  uint64_t sum = 0;

  for (size_t i = 0; i < len; i++) {
    sum = (sum + (num[i] % mod)) % mod;
  }

  return sum;
}
```

Perform modular addition.
