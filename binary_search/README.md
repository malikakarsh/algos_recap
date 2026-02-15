### Binary Search
Built in functions:

lower_bound: First element that is ≥ target. (Greater or Equal)

upper_bound: First element that is > target. (Strictly Greater)

Example: If the list is [2, 5, 5, 5, 8] and your target is 5:

lower_bound points to the first 5.

upper_bound points to the 8 (the first thing strictly bigger than 5).

### 1. The l <= r Template (Searching for a Target)
Use this when you want to find a specific element and stop the moment you find it. This is the most common version.
- **Range:** $[l, r]$ (Both $l$ and $r$ are inclusive).
- **Action:** Inside the loop, you usually have three branches: `==`, `<`, and `>`.
- **Termination:** When $l > r$, the search space is empty.

**Logic:**
```cpp
while (l <= r) {
    int m = l + (r - l) / 2;
    if (nums[m] == target) return m; // Found it!
    else if (nums[m] < target) l = m + 1;
    else r = m - 1;
}
return -1; // Not found
```
**Intuition:** "I am checking every single element. If I haven't found it by the time the range collapses, it’s not there."

### 2. The l < r Template (Finding a Boundary)
Use this when you are looking for a condition or a "pivot point" (like the minimum in a rotated array or the first element that satisfies a property).
- **Range:** $[l, r)$ or $[l, r]$ (Closing the gap).
- **Action:** You usually have two branches. One branch must exclude $m$ (`m + 1` or `m - 1`), and the other must include $m$ (`l = m` or `r = m`).
- **Termination:** The loop stops when $l == r$. Both pointers point to the answer.

**Logic:**
```cpp
while (l < r) {
    int m = l + (r - l) / 2;
    if (condition(m)) r = m; // m could be the answer, so keep it
    else l = m + 1;         // m is definitely not the answer
}
return l; // Both l and r point to the result
```
**Intuition:** "I am narrowing down the possibilities. When the range is only 1 element wide, that's my winner."

### Practice Problems:

- [Different Divisors](https://codeforces.com/problemset/problem/1474/B)
- [Separate Squares I](https://leetcode.com/problems/separate-squares-i/)
- [Find Peak Element](https://leetcode.com/problems/find-peak-element/description/)
- [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
- [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
- [Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/)
- [Sqrt(x)](https://leetcode.com/problems/sqrtx/)
- [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
- [Maximum Capacity Within Budget](https://leetcode.com/problems/maximum-capacity-within-budget/)
- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/description/)
- [Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/)
- [Minimum Number of Days to Make M Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)
- [Longest Strictly Increasing Subsequence With Non-Zero Bitwise AND](https://leetcode.com/problems/longest-strictly-increasing-subsequence-with-non-zero-bitwise-and/description/)
- [Minimum K To Reduce Array Within Limit](https://leetcode.com/problems/minimum-k-to-reduce-array-within-limit/description/)