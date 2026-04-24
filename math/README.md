# Mathematical Algorithms and Properties

## Digital Root (Modulo 9 Property)

### Intuition
The **digital root** of a number is the single digit obtained by repeatedly summing its digits until a single digit remains. For base 10 numbers, there's a direct relationship with modulo 9:

**Key Property**: For any positive integer `n`, `digital_root(n) = 1 + ((n - 1) mod 9)`, or equivalently:
- If `n % 9 == 0` and `n > 0`, then `digital_root(n) = 9`
- Otherwise, `digital_root(n) = n % 9`

**Why this works**: 
- In base 10, `10 ≡ 1 (mod 9)`, so `10^k ≡ 1 (mod 9)` for all `k ≥ 0`
- Therefore, any number `n = d_k·10^k + ... + d_1·10 + d_0` satisfies:
  - `n ≡ d_k + ... + d_1 + d_0 (mod 9)`
- The sum of digits is congruent to the number modulo 9
- Repeatedly summing digits preserves this congruence until we reach a single digit (1-9)

### Code (O(1) solution)
```cpp
int addDigits(int num) {
    if (num == 0) return 0;
    int dr = num % 9;
    return (dr == 0) ? 9 : dr;
}
```

**Alternative iterative approach** (for understanding):
```cpp
int addDigitsIterative(int num) {
    while (num >= 10) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10;
            num /= 10;
        }
        num = sum;
    }
    return num;
}
```

**Time Complexity**: O(1) for the modulo approach, O(log n) for iterative  
**Space Complexity**: O(1)

### Practice Problems:
- [Add Digits](https://leetcode.com/problems/add-digits/)
- [Alternating Digit Sum](https://leetcode.com/problems/alternating-digit-sum/)
- [Average Value Of Even Numbers That Are Divisible By Three](https://leetcode.com/problems/average-value-of-even-numbers-that-are-divisible-by-three/description/)
- [Maximum Matrix Sum](https://leetcode.com/problems/maximum-matrix-sum/)
- [Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points/description/)
- [Hourglass](https://codeforces.com/contest/2184/problem/B)
- [Accomodation](https://www.codechef.com/problems/ACMDT)

---

## Modulo Arithmetic Properties

### Fundamental Property: Congruence Works Bidirectionally
The statement `A ≡ B (mod m) ⟺ A = B + mk` works for **all integers** A and B, regardless of which is larger.

**Key Insight**: The integer `k` can be **negative**, zero, or positive. This means:
- If `A > B`, then `k ≥ 0` (e.g., `7 ≡ 2 (mod 5)` because `7 = 2 + 5·1`)
- If `A < B`, then `k < 0` (e.g., `2 ≡ 7 (mod 5)` because `2 = 7 + 5·(-1)`)
- If `A = B`, then `k = 0`

**Example**: 
- `3 ≡ 8 (mod 5)` because `3 = 8 + 5·(-1)`
- `8 ≡ 3 (mod 5)` because `8 = 3 + 5·1`

Both statements are true, and the congruence relation is symmetric.

### Why Work with Remainders Instead of Large Numbers?

When processing numbers digit-by-digit (especially very large numbers that don't fit in standard integer types), we can work with **remainders modulo m** instead of the actual number.

**Key Property**: If we're building a number `N` digit-by-digit:
- `N = d_k d_{k-1} ... d_1 d_0` (digits from left to right)
- At step `i`, we have prefix `P = d_k ... d_i` and remainder `r = P mod m`
- To add digit `d_{i-1}`, the new number is `P' = P·10 + d_{i-1}`
- The new remainder is: `r' = (P·10 + d_{i-1}) mod m = ((P mod m)·10 + d_{i-1}) mod m = (r·10 + d_{i-1}) mod m`

**Why this works**:
- `(a + b) mod m = ((a mod m) + (b mod m)) mod m`
- `(a·b) mod m = ((a mod m)·(b mod m)) mod m`
- Therefore, `(P·10 + d) mod m = ((P mod m)·10 + d) mod m`

**Example**: Check if `123456789` is divisible by 7
```cpp
// Instead of storing 123456789, we track remainder modulo 7
int remainder = 0;
string num = "123456789";
for (char c : num) {
    int digit = c - '0';
    remainder = (remainder * 10 + digit) % 7;
}
// remainder == 0 means divisible by 7
```

**Benefits**:
1. **Avoids overflow**: Remainders stay bounded (0 to m-1)
2. **Works with string representations**: No need to convert entire number to integer
3. **Efficient**: O(digits) time, O(1) space
4. **Preserves divisibility information**: `N mod m = 0` iff the final remainder is 0

### Practice Problems:
- [Binary Prefix Divisible By 5](https://leetcode.com/problems/binary-prefix-divisible-by-5/)
- [Smallest Integet Divisible By K](https://leetcode.com/problems/smallest-integer-divisible-by-k/)
- [Smallest All-ones Multiple](https://leetcode.com/problems/smallest-all-ones-multiple/)
- [Adding Digits](https://codeforces.com/problemset/problem/260/A)
- [Jzzhu and Sequences](https://codeforces.com/problemset/problem/450/B)
- [Sereja and Bottles](https://codeforces.com/problemset/problem/315/A)
- [Building Permutation](https://codeforces.com/problemset/problem/285/C)
- [Kitahara Haruki's Gift](https://codeforces.com/problemset/problem/433/A)
- [Maximize Area of Square Hole in Grid](https://leetcode.com/problems/maximize-area-of-square-hole-in-grid/)
- [Maximum Square Area by Removing Fences from a Field](https://leetcode.com/problems/maximum-square-area-by-removing-fences-from-a-field/)
- [Find the largest area of square inside two rectangles](https://leetcode.com/problems/find-the-largest-area-of-square-inside-two-rectangles/)
- [Rectangle Area](https://leetcode.com/problems/rectangle-area/description/)
- [Raising Bacteria](https://codeforces.com/problemset/problem/579/A)
- [Balanced Array](https://codeforces.com/problemset/status?my=on)
-[Superultra's Favorite Permutation](https://codeforces.com/problemset/problem/2037/C)
- [Minimize Maximum Pair Sum In Array](https://leetcode.com/problems/minimize-maximum-pair-sum-in-array/)
- [Bestie](https://codeforces.com/problemset/problem/1732/A)
- [Ping Pong](https://codeforces.com/problemset/problem/1455/C)
- [A TRUE Battle](https://codeforces.com/problemset/problem/2030/C)
- [Jumps](https://codeforces.com/contest/1455/problem/B)

## Parity Problems (Identify the parity)
- [Buttons](https://codeforces.com/contest/1858/problem/A)
- [Domino](https://codeforces.com/problemset/problem/353/A)
- [Little Girl and Game](https://codeforces.com/problemset/problem/276/B)
- [Even Modulo Pair](https://codeforces.com/problemset/problem/2164/B)
- [Renako Amaori and XOR Game](https://codeforces.com/problemset/problem/2171/C1)

### Identify the Pattern
```CPP
int clumsy(int n) {
        if (n == 1) return 1;
        if (n == 2) return 2;
        if (n == 3) return 6;
        if (n == 4) return 7;
        if (n%4 == 0) return n+1;
        if (n%4 == 1 || n%4 == 2) return n+2;
        return n-1;
    }
```
- [Clumsy Factorial](https://leetcode.com/problems/clumsy-factorial/description/)
- [Count Good Triplets in an Array](https://leetcode.com/problems/count-good-triplets-in-an-array/)
- [Binary Array Game](https://codeforces.com/problemset/problem/2183/A)
- [Apples In Boxes](https://codeforces.com/problemset/problem/2107/B)
- [Removal Games](https://codeforces.com/problemset/problem/2002/B)
- [Circle Game](https://codeforces.com/problemset/problem/1695/B)
- [Minimum Operations To Make Array Equal](https://leetcode.com/problems/minimum-operations-to-make-array-equal/description/)
- [Minimum Number Of Operations To Make All Array Elements Equal To 1](https://leetcode.com/problems/minimum-number-of-operations-to-make-all-array-elements-equal-to-1/description/)
- [Minimum Cost To Split Into Ones](https://leetcode.com/problems/minimum-cost-to-split-into-ones/)

# Pigeonhole Principle

The Pigeonhole Principle states that if $n$ items are put into $m$ containers, with $n > m$, then at least one container must contain more than one item.

## Guaranteeing k items

If you want to guarantee at least $k$ items in one bucket, and you have $n$ buckets, the number of items you need is:

$$ Items = n(k-1) + 1 $$

## Examples

### 1. The Birth Month Problem
**Problem:** There are 12 months in a year (our "holes"). How many people do you need in a room to guarantee that at least 3 of them were born in the same month?

**Solution:**
Here, $n = 12$ (months) and $k = 3$ (people in same month).
$$ Items = 12(3-1) + 1 = 12(2) + 1 = 25 $$
You need **25 people**.

### 2. The Sock Drawer Problem
**Problem:** You have a drawer full of loose socks: 10 blue socks and 10 black socks. The room is pitch black, and you can't see the colors. How many socks must you grab to be sure you have a matching pair?

**Solution:**
Here, the "holes" are the colors, so $n = 2$ (Blue, Black). We want a pair, so $k = 2$.
$$ Items = 2(2-1) + 1 = 3 $$
You need to pick **3 socks**.

### 3. The Card Sum Problem
**Problem:** You have cards numbered 1 through 12. How many cards must you pick to guarantee a pair that sums to 13?

**Solution:**
First, identify the "holes". The holes are the pairs that sum to 13:
$\{1, 12\}, \{2, 11\}, \{3, 10\}, \{4, 9\}, \{5, 8\}, \{6, 7\}$.
There are $n = 6$ such pairs.
We want to guarantee that we have selected both items from at least one pair. This is a slightly different application. If we pick $n+1$ items, by Pigeonhole Principle, at least two items must belong to the same pair (hole).
Since each "hole" only contains 2 specific cards, finding 2 items in the same hole means we found the pair.
$$ Items = 6(2-1) + 1 = 7 $$
You need to pick **7 cards**.

### Practice Problems:
- [N-Repeated Element in Size 2N Array](https://leetcode.com/problems/n-repeated-element-in-size-2n-array/)


# Prime Checking (6k ± 1 Optimization)

This method allows checking if a number `n` is prime much faster than the naive $O(\sqrt{n})$ loop that increments by 1 or 2.

**Key Insight:** All integers can be expressed as $(6k + i)$, where $i \in \{0, 1, 2, 3, 4, 5\}$.
- $6k + 0, 6k + 2, 6k + 4$ are divisible by 2.
- $6k + 3$ is divisible by 3.
- Thus, all primes greater than 3 must be of the form $6k + 1$ or $6k + 5$ (which is $6k - 1$).

**Why it is faster:**
- We only check potential divisors of the form $6k \pm 1$ ($5, 7, 11, 13, \dots$).
- This reduces the number of trial divisions by a factor of 3 compared to checking all numbers, or effectively better than just checking odd numbers.

**Code:**
```cpp
// Using long long and the 6k+1 optimization for robustness
bool isPrime(long long n) {
    if (n < 2) return false;
    if (n == 2 || n == 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    // We check i and i+2, incrementing by 6
    for (long long i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) return false;
    }
    return true;
}
```
### Practice Problems:
- [Simple Repetition](https://codeforces.com/problemset/problem/2093/C)
- [Minimum LCM](https://codeforces.com/problemset/problem/1765/M)
- [Yet Another Array Problem](https://codeforces.com/problemset/problem/2167/D)
- [The 67th OEIS Problem](https://codeforces.com/contest/2218/problem/D)
- [The 67th Permutation Problem](https://codeforces.com/contest/2218/problem/C)
- [The 67th XOR Problem](https://codeforces.com/contest/2218/problem/E)

# Numbers with Exactly 4 Divisors

A number $n$ has exactly 4 divisors if and only if it falls into one of two cases:

1. **$n$ is the product of two distinct primes ($p \times q$)**
   - The divisors are: $\{1, p, q, p \times q\}$
   - Example: $6 = 2 \times 3$, divisors are $\{1, 2, 3, 6\}$.
   - Example: $10 = 2 \times 5$, divisors are $\{1, 2, 5, 10\}$.

2. **$n$ is the cube of a prime ($p^3$)**
   - The divisors are: $\{1, p, p^2, p^3\}$
   - Example: $8 = 2^3$, divisors are $\{1, 2, 4, 8\}$.
   - Example: $27 = 3^3$, divisors are $\{1, 3, 9, 27\}$.

### Practice Problems:
- [Four Divisors](https://leetcode.com/problems/four-divisors/)
- [T-Primes](https://codeforces.com/problemset/problem/230/B)

# GCD and LCM Relation

For any two positive integers $a$ and $b$, there is a fundamental relationship between their Greatest Common Divisor (GCD) and Least Common Multiple (LCM):

$$ \gcd(a, b) \times \text{lcm}(a, b) = |a \cdot b| $$

This allows us to compute the LCM efficiently using the GCD:

$$ \text{lcm}(a, b) = \frac{|a \cdot b|}{\gcd(a, b)} $$

To avoid potential overflow when computing $a \cdot b$, it is safer to compute it as:
`lcm(a, b) = (a / gcd(a, b)) * b`

**C++ Note:**
- `std::gcd(a, b)` and `std::lcm(a, b)` are available in the `<numeric>` header (C++17).

### Practice Problems:
- [Yet Another Permutation Problem](https://codeforces.com/problemset/problem/1858/C)
- [String LCM](https://codeforces.com/problemset/problem/1473/B)

# Bézout's Identity

**Bézout's Identity** states that for any integers $a$ and $b$ (not both zero), there exist integers $x$ and $y$ such that:

$$ ax + by = \gcd(a, b) $$

- The equation $ax + by = k$ has integer solutions $(x, y)$ if and only if $k$ is a multiple of $\gcd(a, b)$.
- **Note:** The integers $x$ and $y$ can be **negative**. If there is a constraint that $x$ and $y$ must be positive (or non-negative), then this guarantee does not hold.
- These coefficients $(x, y)$ can be found using the **Extended Euclidean Algorithm**.

# Frobenius Coin Problem (Chicken McNugget Theorem)

The **Frobenius Coin Problem** asks for the largest monetary amount that **cannot** be obtained using only coins of specified denominations. For two denominations $a$ and $b$ that are **relatively prime** (or **coprime**):

> **Definition:** Two numbers are **relatively prime** (or **coprime**) if their Greatest Common Divisor (GCD) is 1 (e.g., $\gcd(a, b) = 1$).

1. **Largest Impossible Amount (Frobenius Number):**
   $$ g(a, b) = ab - a - b $$

2. **Number of Impossible Amounts:**
   $$ N(a, b) = \frac{(a-1)(b-1)}{2} $$

### Example
For coins of value 3 and 5:
- Largest Impossible Amount: $3 \times 5 - 3 - 5 = 15 - 8 = 7$.
- Amounts that cannot be formed: 1, 2, 4, 7. (Total 4, which matches $\frac{(3-1)(5-1)}{2} = \frac{2 \times 4}{2} = 4$).

### Practice Problems:
- [Social Experiment](https://codeforces.com/contest/2184/problem/A)
- [Good ol' Numbers Coloring](https://codeforces.com/problemset/problem/1245/A)

## Reachability In 2D Grids

### Problem A: The Buffer Overflow Jump

A penetration tester is trying to exploit a vulnerability by carefully controlling a memory pointer. The pointer starts at a base address conceptualized as a 2D grid coordinate (0,0). To bypass the system's security checks, the pointer can only be incremented using three specific operational codes (opcodes).

Each opcode shifts the pointer's $(x,y)$ coordinates in the following ways:
- **Opcode 1**: $(x_i, y_i) \to (x_i + 1, y_i + 2)$
- **Opcode 2**: $(x_i, y_i) \to (x_i + 2, y_i + 1)$
- **Opcode 3**: $(x_i, y_i) \to (x_i + 3, y_i + 3)$

Determine if it is possible to reach the target address $(X, Y)$ using any combination of these opcodes, starting from $(0, 0)$ without ever jumping to a negative coordinate.

**The Solution:**
1. **Divisibility:** Notice that for all opcodes, the sum of increments (1+2, 2+1, 3+3) is a multiple of 3. Thus, $X+Y$ must be divisible by 3.
2. **Linear Combination:** Notice that Opcode 3 is simply $(Opcode 1 + Opcode 2)$. Thus, we only need to check if $X$ and $Y$ can be formed by non-negative integers $a$ (Opcode 1) and $b$ (Opcode 2):
   - $a + 2b = X$
   - $2a + b = Y$
3. **Solving for $a, b$:**
   - $b = \frac{2X - Y}{3}$
   - $a = \frac{2Y - X}{3}$
4. **Boundary Conditions:** For $a, b \ge 0$, we must have $2X \ge Y$ and $2Y \ge X$.

**Final O(1) Check:**
- `(X + Y) % 3 == 0` AND `2*X >= Y` AND `2*Y >= X` $\implies$ **YES**

---

### Problem B: The Dream Architect

You start at $(0,0)$ and want to reach $(X,Y)$ using three specific movement patterns:
- **Shift 1**: $(x_i, y_i) \to (x_i + 2, y_i)$
- **Shift 2**: $(x_i, y_i) \to (x_i, y_i + 2)$
- **Shift 3**: $(x_i, y_i) \to (x_i + 1, y_i + 1)$

Determine if it is possible to reach $(X,Y)$ starting from $(0,0)$.

**The Solution:**
1. **Parity:** Notice that all shifts $(+2,0), (0,+2), (+1,+1)$ preserve the parity of $X+Y$. Specifically, $X$ and $Y$ must have the same parity (both even or both odd), which means $X+Y$ must be even.
2. **Linear Combination:**
   - $2a + c = X$
   - $2b + c = Y$
   If $X \equiv Y \pmod 2$, we can always find non-negative $a, b, c$.
3. **Traps:** Target coordinates can be negative. Since all shifts only increase coordinates, $X$ and $Y$ must be non-negative.

**Final O(1) Check:**
- `(X % 2 == Y % 2)` AND `X >= 0` AND `Y >= 0` $\implies$ **YES**

### Practice Problems:
- [Parkour Design](https://codeforces.com/contest/2202/problem/A)
- [Count Commas in Range](https://leetcode.com/problems/count-commas-in-range/)
- [Count Commas in Range II](https://leetcode.com/problems/count-commas-in-range-ii/description/)