# Multiples of 3 and 5

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.

## Solution

It is possible to fund the sum of all multiples of $n$ below $m$ using a formula.

To find the sum of all multiples of $n$ below $m$, the individual multiples have to be summed up.

$$
1 * n + 2 * n + 3 * n + \dots x * n
$$

Here, $x * n$ is the largest multiple that is lower than $m$, so it can be said that

$$
x = \left \lfloor\frac{x - 1}{n}\right \rfloor
$$

This can be rearranged such.

$$
(1 + 2 + 3 + \dots x) * n
$$

The Gaußian sum formula can be used to simplify this.

$$
n * \frac{x^2 + x}{2}
$$

### C

```c
uint32_t sum_divisible(uint32_t max, uint32_t divisor) {
  // smallest number lower than max that is
  // divisible by divisor
  int bound = max - (max % divisor);

  return (bound * ((bound / divisor) + 1)) / 2;
}
```

```c
uint32_t sum_multiples(uint32_t max, uint32_t div1, uint32_t div2) {
  return sum_divisible(max, div1) + sum_divisible(max, div2) -
         sum_divisible(max, lcm(div1, div2));
}
```
