## Sliding Window and Prefix Sum (with C++)

### What is Sliding Window?
Sliding window is a pattern to process contiguous subarrays efficiently by moving two indices over the array. Instead of recomputing from scratch, we update the current window state when the window shifts.

- **Fixed-size window**: Window length is constant (e.g., size `k`).
- **Variable-size window (two pointers)**: Window expands and shrinks based on a condition (e.g., sum constraint).

---

### Fixed-size Window: Maximum sum of any subarray of size k
Keep a rolling sum: add the entering element and subtract the leaving element as the window slides.

```cpp
#include <bits/stdc++.h>
using namespace std;

long long maxSumOfSizeK(const vector<int>& nums, int k) {
    if (k <= 0 || k > (int)nums.size()) return 0;
    long long windowSum = 0, best = LLONG_MIN;
    for (int i = 0; i < (int)nums.size(); i++) {
        windowSum += nums[i];
        if (i >= k) windowSum -= nums[i - k];
        if (i >= k - 1) best = max(best, windowSum);
    }
    return best;
}

int main() {
    vector<int> a{2, 1, 5, 1, 3, 2};
    int k = 3;
    cout << maxSumOfSizeK(a, k) << "\n"; // Walkthrough: windows [2,1,5]=8, [1,5,1]=7, [5,1,3]=9, [1,3,2]=6 → answer 9
}
```

Walkthrough for `a = [2,1,5,1,3,2], k = 3`:
- Start sum = 2+1+5 = 8
- Slide: add `1`, remove `2` → 7
- Slide: add `3`, remove `1` → 9 (best)
- Slide: add `2`, remove `5` → 6

### Practice Problem:
- [Maximum Sum Of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/submissions/1773207847/)
---

### Variable-size Window: Smallest length subarray with sum ≥ target
Expand `right` to increase sum; shrink from `left` while the condition holds to minimize length.

```cpp
#include <bits/stdc++.h>
using namespace std;

int minLenAtLeastTarget(const vector<int>& nums, long long target) {
    int n = (int)nums.size();
    int left = 0, best = INT_MAX;
    long long sum = 0;
    for (int right = 0; right < n; right++) {
        sum += nums[right];
        while (sum >= target) {
            best = min(best, right - left + 1);
            sum -= nums[left++];
        }
    }
    return best == INT_MAX ? 0 : best;
}

int main() {
    vector<int> a{2, 3, 1, 2, 4, 3};
    cout << minLenAtLeastTarget(a, 7) << "\n"; // Answer 2 (subarray [4,3])
}
```

Walkthrough for `a = [2,3,1,2,4,3], target = 7`:
- Expand until sum ≥ 7 at `[2,3,1,2]` length 4 → shrink to try shorter → `[3,1,2]` sum 6, stop shrink
- Expand to include `4`: sum 10 → shrink to `[4,3]` length 2 → best = 2

## Practice Problem:
- [Maximum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
---

### Using Prefix Sum with Sliding Window
Prefix sums let you get any window sum in O(1): `sum(l..r) = pref[r+1] - pref[l]` where `pref[0] = 0` and `pref[i+1] = pref[i] + nums[i]`.

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<long long> buildPrefix(const vector<int>& nums) {
    vector<long long> pref(nums.size() + 1, 0);
    for (int i = 0; i < (int)nums.size(); i++) pref[i + 1] = pref[i] + nums[i];
    return pref;
}

long long windowSumByPrefix(const vector<long long>& pref, int l, int r) {
    // assumes 0 <= l <= r < n
    return pref[r + 1] - pref[l];
}

int main() {
    vector<int> a{2, 1, 5, 1, 3, 2};
    auto pref = buildPrefix(a);
    cout << windowSumByPrefix(pref, 2, 4) << "\n"; // sum of [5,1,3] = 9
}
```

- **Rolling vs Prefix:** For fixed-size windows, a rolling sum avoids extra memory. For many arbitrary window queries, prefix sums are ideal.

---

### Range Increment/Decrement via Prefix Sums (Difference Array)
To apply many subarray updates efficiently (increment or decrement each element in a range), use a difference array `diff` and a final prefix sum to materialize the result.

Concept for an update `(l, r, delta)` on 0-indexed inclusive range:
- `diff[l] += delta`
- `diff[r + 1] -= delta` (if `r + 1 < n`)
- After processing all updates, compute `arr[i] = base[i] + prefixSum(diff)[i]`

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<long long> applyRangeUpdates(const vector<long long>& base,
                                    const vector<tuple<int,int,long long>>& updates) {
    int n = (int)base.size();
    vector<long long> diff(n + 1, 0);
    for (auto [l, r, delta] : updates) {
        if (l < 0) l = 0; if (r >= n) r = n - 1; if (l > r) continue;
        diff[l] += delta;
        if (r + 1 < n) diff[r + 1] -= delta;
    }
    vector<long long> result(n, 0);
    long long run = 0;
    for (int i = 0; i < n; i++) {
        run += diff[i];
        result[i] = base[i] + run;
    }
    return result;
}

int main() {
    vector<long long> base{1, 0, 0, 0, 0, 0, 0, 1};
    vector<tuple<int,int,long long>> updates = {
        {1, 3, +2},  // increment subarray [1..3] by 2
        {2, 5, -1},  // decrement subarray [2..5] by 1
        {0, 0, +5}   // increment index 0 by 5
    };
    auto out = applyRangeUpdates(base, updates);
    for (auto x : out) cout << x << ' '; // Expected: 6 2 1 1 -1 -1 0 1
    cout << "\n";
}
```

Walkthrough of the updates step-by-step:
- Start `base = [1,0,0,0,0,0,0,1]`
- Apply `{1,3,+2}` → positions 1..3 get +2
- Apply `{2,5,-1}` → positions 2..5 get -1
- Apply `{0,0,+5}` → position 0 gets +5
- Final array after prefixing `diff`: `[6,2,1,1,-1,-1,0,1]`

Why it works:
- The difference array stores how much the running sum changes at indices. Taking a prefix sum reconstructs the per-index total delta from overlapping range updates.

---

### When to Combine Sliding Window and Prefix Sums
- **Many window queries after range updates**: Use the difference array to apply all updates, reconstruct, then answer window sums by prefix in O(1) per query.
- **Constraints on sum while scanning once**: Use a variable-size sliding window; optionally maintain sums with a running counter (equivalent to subtracting/adding like a rolling prefix).

Complexities:
- Fixed/variable sliding window scans: O(n)
- Building prefix: O(n); window sum by prefix: O(1)
- Applying m range updates (difference array): O(n + m)

### Practice Problems:
- [Remove Nth Node From End Of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/submissions/1773234498/?envType=problem-list-v2&envId=linked-list)
