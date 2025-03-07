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

## Subtraction Property

If for example, using 7, **~={red}x % 7 = y % 7=~**, then **~={red}(x - y) % 7 = 0=~**

For example:
* 17 % 7 = 3
* 10 % 7 = 3

= 17 - 10                
= (14 + 3) - (7 + 3) = **~={green}(14 - 7)=~** + **~={red}(3 - 3)=~** = 7

The offset is cancelled out, leaving the factors of 7 present. Hence doing mod on the result is just 0

## Number of Factors of X in n!

If we wanted to find out how many of factors of X in n!:

$$\frac{n}{x} + \frac{n}{x^2} + \frac{n}{x^3} + ...$$
For example, the number of factors of 5 in 30!, come from:
* 5, 10, 15, 20, 25, 30 } $30 / 5 = 6$
* 25 } $30 / 25 = 1$
* Hence total is 7

## List of All Divisors/Factors

```cpp
int n = 20;

for (int i{1}; i <= sqrt(20); ++i) {
	if (n % i == 0) {
		int divisorOne = i;
		int divisorTwo = n / i;
		
		// Do Something with Divisor One
		
		// Do Somethig with Divisor Two if Divisor One != Divisor Two
		if (divisorOne != divisorTwo && ...)
	}
}
```

## GCD and LCM

**When One is Involved**

* GCD(1, b) = 1
* LCM(a, b) = b

## Convert Decimal to Base X

Note that i.e. base 3:
* Decimal Number 19 = $201_3 = 2 \cdot 3^2 + 0 \cdot 3^1 + 1 \cdot 3^0$
From this we can infer if more than 1 power of 3 has been used to construct a number.

```cpp
int n{10};

string ternaryRep{};

while (n > 0) {
	// Remainder when dividng by 3
	// Keep adding to the front of the existing string
	ternaryRep = to_string(n % 3) + ternaryRep;
	n /= 3;
}
```

## All Primes Between 0 and Limit

We can use something called the **~={blue}Sieve of Eratosthenes=~**.

![[Agb5mPVOVZ-sieve_of_eratosthenes_animation.gif | center | 350]]

```cpp
vector<bool> isPrime(limit + 1, true);
isPrime[0] = false;
isPrime[1] = false;

for (int i{2}; i * i <= limit; ++i) {
	if (isPrime[i] == true) {
		for (int j{i * i}; j <= limit; j += i) {
			isPrime[j] = false;
		}
	}
}

vector<int> primes;

for (int i{0}; i < isPrime.size(); ++i) {
	if(isPrime[i] == true) primes.push_back(i);
}
```

# Bit Manipulation

## Binary Numbers


## Common Bitwise Operations

| Operation   | Symbol | Description                                                                                                                                              | Example (A = 5, B = 3)                           |
| ----------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| AND         | `&`    | Sets each bit to 1 if both bits are 1                                                                                                                    | `5 & 3` → `0101 & 0011 = 0001 (1)`               |
| OR          | `\|`   | Sets each bit to 1 if either bit is 1                                                                                                                    | `5 \| 3` → `0101 \| 0011 = 0111 (7)`             |
| XOR         | `^`    | 1 if the 2 bits are different. 0 if the same.<br>~={red}(If the number of `1` bits is odd, then the result will be `1`. Otherwise, the result is `0`.)=~ | `5 ^ 3` → `0101 ^ 0011 = 0110 (6)`               |
| NOT         | `~`    | Inverts all bits                                                                                                                                         | `~5` → `~(0101) = 1010 (-6 in two's complement)` |
| Left Shift  | `<<`   | Shifts bits left, filling with 0s (x 2)                                                                                                                  | `5 << 1` → `0101 << 1 = 1010 (10)`               |
| Right Shift | `>>`   | Shifts bits right, preserving sign for signed numbers (/ 2)                                                                                              | `5 >> 1` → `0101 >> 1 = 0010 (2)`                |

## Truth Tables

### Common Bitwise Operations and Truth Table

| A | B | A & B (AND) | A \| B (OR) | A ^ B (XOR) |
|---|---|-----------|-----------|-----------|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |

---

### Bitwise NOT (`~`)

| A | ~A (Flipped) |
|---|------------|
| 0 | 1 |
| 1 | 0 |

(Note: In C++, `~A = -A - 1` due to two’s complement representation.)

---

### Left and Right Shift Examples

| A (Decimal) | A (Binary) | A << 1 (Left Shift) | A >> 1 (Right Shift) |
| ----------- | ---------- | ------------------- | -------------------- |
| 1           | 0001       | 0010 (2)            | N/A                  |
| 2           | 0010       | 0100 (4)            | 0001 (1)             |
| 4           | 0100       | 1000 (8)            | 0010 (2)             |
| 8           | 1000       | 0001 0000 (16)      | 0100 (4)             |
#### Why a Left Shift is Equivalent to Multiplying by 2

When you shift left, **~={red}every bit=~** moves **one position to the left**, meaning for i.e. ABC -> ABC0 

* What was in the **2⁰ (ones place)** moves to the **2¹ (twos place)** **~={green}} C moved to where B was=~**
* What was in the **2¹ (twos place)** moves to the **2² (fours place)** **~={green}} B moved to where A was=~**
* What was in the **2² (fours place)** moves to the **2³ (eights place)** **~={green}} A up by a new place value=~**

Since every bit now represents **twice its previous value**, the number effectively doubles. (In base 10, if we doubled each "bit", 123 -> 246, which is double)

## Check if a number is a power of 2

![[Pasted image 20250204214635.png]]

# Geometry

## Number of A x B Rectangles in a Grid

For a `2 x 3` square in a `n = 4 x m = 4` grid:
![[Drawing 2025-01-18 08.54.15.excalidraw | center | 650]]