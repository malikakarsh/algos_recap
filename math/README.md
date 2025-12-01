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


