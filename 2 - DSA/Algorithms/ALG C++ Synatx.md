# Arrays

## Looping Through an Array to Find a Value ->

* Start at position X, and arrive back at position X 

```cpp
int n = arr.size();
for (int i = start; i != destination; i = (i + 1) % n) {}
```

# Number Theory

## Is it possible to partition the sum of the first n integers into 2 sets

* ~={purple}**If ODD**=~: It is ~={red}not possible=~
* **~={purple}If Even=~**: Partition **~={yellow}may be possible=~**.
	* Start by placing the largest number $n$ in one set.
	* Work greedily by alternately adding the next largest number to the sets to balance their sums.

## Distributive Property of Modular Arithmetic

| **~={purple}Addition=~**                                        | **~={purple}Subtraction=~**                                     | **~={purple}Multiplication=~**                                          |
| --------------------------------------------------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------- |
| $(a + b) \; \% \; m = (a \; \% \; m + b \; \% \; m) \; \% \; m$ | $(a - b) \; \% \; m = (a \; \% \; m - b \; \% \; m) \; \% \; m$ | $(a \cdot b) \; \% \; m = (a \; \% \; m \cdot b \; \% \; m) \; \% \; m$ |

**~={red}For example=~**, `(3 * 4 * 5) % 10^9 + 7`
* To prevent overflow, we can % our intermediate results as well
* This is allowed, as i.e. 12 + 33 + 79 = 124, where 124 % 10 = 4.
	* (12 % 10 + 33 % 10) % 10 = 5
	* (5 % 10 + 79 % 10) % 10 = 4, as such, because we only care about the last digit here, it doesn't matter if we lose extra information.

```cpp
const int MOD = 1e9 + 7;
long long result = 1;

for (int i{3}; i <= 5; ++i) {
	result = ((result % MOD) * (i % MOD)) % MOD;
}
```

## Number of Factors of X in n!

If we wanted to find out how many of factors of X in n!:

$$\frac{n}{x} + \frac{n}{x^2} + \frac{n}{x^3} + ...$$
For example, the number of factors of 5 in 30!, come from:
* 5, 10, 15, 20, 25, 30 } $30 / 5 = 6$
* 25 } $30 / 25 = 1$
* Hence total is 7

# Geometry

## Number of A x B Rectangles in a Grid

For a `2 x 3` square in a `n = 4 x m = 4` grid:
![[Drawing 2025-01-18 08.54.15.excalidraw | center | 650]]