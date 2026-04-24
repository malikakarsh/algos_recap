## Bit Manipulation Tricks

The result of $ans \text{ OR } (ans + 1)$ is that it takes the original number and fills in the rightmost zero.

### `__builtin_popcount` and Custom Sorting

The `__builtin_popcount(x)` function counts the number of set bits (1s) in an integer. Its time complexity is $O(1)$ on most modern architectures because it is typically implemented using a dedicated hardware instruction (like `POPCNT` on x86).

Here is an example of sorting an array of integers by their number of set bits (and by their value in case of a tie):

```cpp
static bool myComp(int num1, int num2) {
    int setBits1 = __builtin_popcount(num1);
    int setBits2 = __builtin_popcount(num2);
    return (setBits1 == setBits2) ? num1 < num2 : setBits1 < setBits2;
}

vector<int> sortByBits(vector<int>& arr) {
    sort(arr.begin(), arr.end(), myComp);
    return arr;
}
```

#### The Power of the Signature
The signature `bool myComp(int num1, int num2)` tells the manager two things:

- **The Inputs**: "I will give you two numbers from the list, one as `num1` and one as `num2`."
- **The Output**: "You must tell me `true` if `num1` should stay to the left of `num2`, or `false` if it shouldn't."

### Concatenation of Consecutive Binary Numbers

When we need to concatenate the binary representations of integers from $1$ to $n$ and find the modulo, we have three main approaches:

**Approach 1:** An $O(n \log n)$ string/vector-based approach where we start with $i=n$ all the way down to $i=1$. For each $i$, we extract its binary digits and append them to a vector or string, then compute the final decimal result from that concatenated representation.

**Approach 2:** We iterate from $i=1$ to $n$, and for each step, we shift our running result (`res`) left by the number of bits required to represent $i$ (`space`), and then add $i$.
```cpp
int concatenatedBinary(int n) {
    long long res = 0;
    const int mod = 1e9+7;
    for (int i = 1; i <= n; i++) {
        int space = log2(i) + 1;
        res = ((res << space) % mod + i % mod) % mod;
    }
    return res;
}
```

**Approach 3 (Optimized):** Instead of using `log2(i) + 1` to find the number of bits for every integer, we can keep track of the bit length. The number of bits (`digits`) increases by 1 only when $i$ is a power of 2. We can check if a number is a power of 2 efficiently using `(i & (i - 1)) == 0`. If this is zero, it means it is a power of 2, in which case we do `digits++`. This eliminates the expensive `log2` calculations, resulting in a robust bit manipulation solution.

### Practice Problems:
- [Concatenation of Consecutive Binary Numbers](https://leetcode.com/problems/concatenation-of-consecutive-binary-numbers/)
- [Sort Integers by The Number of 1 Bits](https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/)
- [Construct the Minimum Bitwise Array I](https://leetcode.com/problems/construct-the-minimum-bitwise-array-i/)
- [Single Number](https://leetcode.com/problems/single-number/)
- [Single Number II](https://leetcode.com/problems/single-number-ii/)
- [Single Number III](https://leetcode.com/problems/single-number-iii/submissions/)
- [Construct the Minimum Bitwise Array II](https://leetcode.com/problems/construct-the-minimum-bitwise-array-ii/)
- [Prime Number of Set Bits in Binary Representation](https://leetcode.com/problems/prime-number-of-set-bits-in-binary-representation/)
- [Binary Gap](https://leetcode.com/problems/binary-gap/)
- [Maximum Bitwise XOR After Rearrangement](https://leetcode.com/problems/maximum-bitwise-xor-after-rearrangement/description/)
- [Check If a String Contains All Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)
- [Sum of Root to Leaf Binary Numbers](https://leetcode.com/problems/sum-of-root-to-leaf-binary-numbers/)
- [Number of Steps to Reduce a Number in Binary Representation to One](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-in-binary-representation-to-one/)
- [Partitioning Into Minimum Number Of Deci-Binary Numbers](https://leetcode.com/problems/partitioning-into-minimum-number-of-deci-binary-numbers/)
- [Find Kth Bit in Nth Binary String](https://leetcode.com/problems/find-kth-bit-in-nth-binary-string/)
- [Find Unique Binary String](https://leetcode.com/problems/find-unique-binary-string/)
- [Minimum Number of Flips to Make the Binary String Alternating](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/)
- [Complement of Base 10 Integer](https://leetcode.com/problems/complement-of-base-10-integer/)
- [Number Complement](https://leetcode.com/problems/number-complement/)
- [Walking Robot Simulation II](https://leetcode.com/problems/walking-robot-simulation-ii/)