### Practice Problems:
- [Rotate Array](https://leetcode.com/problems/rotate-array/)
- [Move Zeroes](https://leetcode.com/problems/move-zeroes/)
- [Missing Numbers](https://leetcode.com/problems/missing-number/)
- [Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/)

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


