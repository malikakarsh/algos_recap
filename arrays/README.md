### Practice Problems:
- [Rotate Array](https://leetcode.com/problems/rotate-array/)
- [Rotate Non-Negative Elements](https://leetcode.com/problems/rotate-non-negative-elements/description/)
- [Add Binary](https://leetcode.com/problems/add-binary/description/)
- [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
- [Missing Numbers](https://leetcode.com/problems/missing-number/)
- [Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/)
- [Longest Even odd Subarray With Threshold](https://leetcode.com/problems/longest-even-odd-subarray-with-threshold/description/)
- [Arrival Of The General](https://codeforces.com/problemset/problem/144/A)
- [Laptops](https://codeforces.com/problemset/problem/456/A)
- [Pashmak and Garden](https://codeforces.com/problemset/problem/459/A)
- [Longest Subarray With Sum Divisible By K](https://leetcode.com/problems/maximum-subarray-sum-with-length-divisible-by-k/description/)
- [Largest Magic Square](https://leetcode.com/problems/largest-magic-square/)
- [Minimum Pair Removal To Sort Array II](https://leetcode.com/problems/minimum-pair-removal-to-sort-array-ii/)
- [Minimum Prefix Removal To Make Array Strictly Increasing](https://leetcode.com/problems/minimum-prefix-removal-to-make-array-strictly-increasing/description/)
- [Divide An Array Into Subarrays With Minimum Cost I](https://leetcode.com/problems/divide-an-array-into-subarrays-with-minimum-cost-i/)
- [Sequential Nim](https://codeforces.com/contest/1382/problem/B)
- [Matrix Game](https://codeforces.com/problemset/problem/1365/A)
- [Trouble Sort](https://codeforces.com/contest/1365/problem/B)
- [Trionic Array I](https://leetcode.com/problems/trionic-array-i/)
- [Transformed Array](https://leetcode.com/problems/transformed-array/description/)
- [Bitwise XOR of All Pairings](https://leetcode.com/problems/bitwise-xor-of-all-pairings/)
- [Longest Balanced Subarray I](https://leetcode.com/problems/longest-balanced-subarray-i/)
- [Longest Balanced Substring I](https://leetcode.com/problems/longest-balanced-substring-i/)

---

## Moore's Voting Algorithm

### Majority Element (> ⌊n/2⌋ occurrences)
Intuition: If an element is a true majority, we can pair each occurrence of it with a different element; after canceling all such pairs, the majority survives as the last candidate.

Algorithm (two passes):
1) Candidate selection: scan once, maintain `(cand, count)`. If `count==0`, set `cand=a[i]`, `count=1`; else `count += (a[i]==cand ? 1 : -1)`.
2) Verification: scan again and count occurrences of `cand`; it’s majority iff `count > n/2`.

Time O(n), space O(1).

```cpp
int majorityElement(const vector<int>& a) {
    int cand = 0, cnt = 0;
    for (int x : a) {
        if (cnt == 0) { cand = x; cnt = 1; }
        else cnt += (x == cand ? 1 : -1);
    }
    // optional verification if the array might not have a majority
    int occ = 0; for (int x : a) if (x == cand) ++occ;
    return (occ > (int)a.size() / 2) ? cand : INT_MIN; // or flag as "none"
}
```

### Generalization: Elements Occurring > ⌊n/k⌋ Times
Maintain up to `k−1` candidates with counts. For each value:
- If it matches a stored candidate, increment its count.
- Else if there’s a free slot, insert it with count 1.
- Else decrement all counts by 1 (cancelling `k` distinct elements at once).
Finally, verify the candidates by counting occurrences in a second pass.

Example for `k=3` (popular ≥ n/3): keep up to 2 candidates, then verify. This underpins the “popular widgets” problem.

### Practice Problems:
- [Majority Element](https://leetcode.com/problems/majority-element/)
- [Majority Element II](https://leetcode.com/problems/majority-element-ii/)
- [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)


---

## Kadane's Algorithm – Maximum Subarray Sum (O(n))

Intuition: The best subarray ending at position i either extends the best subarray ending at i−1 (add `nums[i]`) or starts fresh at `nums[i]` if the previous sum is negative.

```cpp
int maxSubArray(const vector<int>& nums) {
    int curr = nums[0];
    int best = nums[0];
    for (int i = 1; i < (int)nums.size(); ++i) {
        curr = max(curr + nums[i], nums[i]);
        best = max(best, curr);
    }
    return best;
}
```

Notes:
- Handles negative numbers as well; initialize with `nums[0]`.
- If you need the subarray itself, track start/end indices when `curr` is reset.

### Practice Problems:
- [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- [Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)
---

## Maximum Product Subarray (Kadane variant)

### Intuition
- Multiplying by a negative flips maxima and minima. So we must carry both the best and worst products ending at the current index.
- When `nums[i] < 0`, swap the running `max` and `min` before updating because a negative number will turn the smallest (most negative) product into the largest, and vice versa.
- Zeros naturally reset the running product since `max(nums[i], curMax*nums[i])` will pick `nums[i]` (0) when appropriate.

### Code (C++)
```cpp
int maxProduct(const vector<int>& nums) {
    int n = (int)nums.size();
    int curMax = nums[0];
    int curMin = nums[0];
    int best   = nums[0];

    for (int i = 1; i < n; ++i) {
        if (nums[i] < 0) swap(curMax, curMin);
        curMax = max(nums[i], curMax * nums[i]);
        curMin = min(nums[i], curMin * nums[i]);
        best = max(best, curMax);
    }
    return best;
}
```



---

## Find Duplicate Number (Pigeonhole Principle)

**Problem:** Given an array of size $n+1$ with integers in range $[1, n]$, find the duplicate number (guaranteed to exist).

### Method 1: Floyd's Algorithm (Cycle Detection) - Best for Read-Only
View the array as a linked list where `nums[i]` points to `nums[nums[i]]`. Since there is a duplicate, there is a cycle.
- **Phase 1:** Use solution pointers (tortoise/hare) to find an intersection point.
- **Phase 2:** Reset one pointer to start and move both one step at a time to find the cycle entrance (the duplicate).

**Complexity:** $O(n)$ Time, $O(1)$ Space. Does not modify array.

```cpp
int findDuplicate(vector<int>& nums) {
    int slow = nums[0];
    int fast = nums[0];
    
    // Phase 1: Intersection
    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);
    
    // Phase 2: Cycle Entrance
    slow = nums[0];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```

### Method 2: Negative Marking - Best if Modification Allowed
Since numbers are in $[1, n]$, we can use them as indices.
- Iterate through the array. For each number `val = abs(nums[i])`:
    - Check the value at index `val`.
    - If `nums[val]` is already **negative**, then `val` has been seen before → **Duplicate found**.
    - Otherwise, negate `nums[val]` to mark it found.

**Complexity:** $O(n)$ Time, $O(1)$ Space. Modifies array (can be restored).

```cpp
int findDuplicateMarking(vector<int>& nums) {
    for (int i = 0; i < nums.size(); ++i) {
        int idx = abs(nums[i]); // Use value as index
        if (nums[idx] < 0) return idx; // Already marked
        nums[idx] = -nums[idx]; // Mark visited
    }
    return -1;
}
```

### Practice Problems:
- [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/description/)

---

## Reduced Prefix Sum Strategy (Variable Reduction)

You can suspect this method if the problem involves these specific keywords:
- "Equal number of..." $\to$ Look for $Sum = 0$.
- "More than..." or "Majority..." $\to$ Look for $Sum > 0$.
- "At least $K$ more than..." $\to$ Look for $Sum \ge K$.

### The Logic Chain
1. **Count Constraint:** $Count(A) > Count(B)$.
2. **Subtraction:** $Count(A) - Count(B) > 0$.
3. **Transformation:** Replace $A$ with $+1$ and $B$ with $-1$. The expression $Count(A) - Count(B)$ is now just the sum of the array.

### Ask yourself if this trick works for these scenarios:
1. **Subarrays with equal number of Even and Odd numbers?**
   - **Yes:** Even $\to 1$, Odd $\to -1$. Find $Sum = 0$.
2. **Subarrays where the number of vowels is greater than consonants?**
   - **Yes:** Vowel $\to 1$, Consonant $\to -1$. Find $Sum > 0$.
3. **Subarrays where the number of $5$s is exactly double the number of $3$s?**
   - **Yes (but trickier):** $5 \to +1$, $3 \to -2$. Find $Sum = 0$.
   - **Why?** Since $1 \times Count(5) - 2 \times Count(3) = 0$ simplifies to $Count(5) = 2 \times Count(3)$.

### Practice Problems:
- [Contiguous Array](https://leetcode.com/problems/contiguous-array/description/)
- [Count Subarrays With More Ones Than Zeros](http://algo.monster/liteproblems/2031)
- [Count Subarrays With Majority Element I](https://leetcode.com/problems/count-subarrays-with-majority-element-i/)
- [Count Subarrays With Majority Element II](https://leetcode.com/problems/count-subarrays-with-majority-element-ii/)