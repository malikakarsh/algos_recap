### Practice Problems:
- [Count Triplets with Sum Smaller Than X](https://www.geeksforgeeks.org/problems/count-triplets-with-sum-smaller-than-x5549/1)
- [4Sum](https://leetcode.com/problems/4sum/)
- [3Sum With Multiplicity](https://leetcode.com/problems/3sum-with-multiplicity/submissions/)
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

---

## 3Sum With Multiplicity – Two-Pointer Counting with Duplicates

### Intuition
- Sort the array. For each fixed index `i`, we want the number of pairs `(l, r)` (with `l > i`) such that `arr[i] + arr[l] + arr[r] == target`.
- Move `l` and `r` with the classic two-pointer technique.
- When `arr[i] + arr[l] + arr[r] < target`, increase `l`; when greater, decrease `r`.
- When equal, we must count how many equal copies are clustered at `l` and at `r`:
  - If `arr[l] != arr[r]`, let there be `lc` equal values at `l` and `rc` equal values at `r`; we add `lc * rc` triplets and skip those blocks.
  - If `arr[l] == arr[r]`, then the whole window `[l..r]` is the same value; if its length is `m = r − l + 1`, the number of distinct pairs is `C(m,2) = m*(m−1)/2`. Add that and break the loop for this `i`.
- Take modulo `1e9+7` for the result.

### Code (C++)
```cpp
class Solution {
public:
    int threeSumMulti(vector<int>& arr, int target) {
        const long long MOD = 1'000'000'007LL;
        sort(arr.begin(), arr.end());
        int n = (int)arr.size();
        long long ans = 0;

        for (int i = 0; i < n - 2; ++i) {
            int l = i + 1, r = n - 1;
            while (l < r) {
                long long s = (long long)arr[i] + arr[l] + arr[r];
                if (s < target) {
                    ++l;
                } else if (s > target) {
                    --r;
                } else { // s == target
                    if (arr[l] == arr[r]) {
                        long long m = r - l + 1;                  // all equal in [l..r]
                        ans = (ans + m * (m - 1) / 2) % MOD;      // choose 2
                        break;                                     // all pairs in this segment counted
                    } else {
                        long long lc = 1, rc = 1;
                        while (l + 1 < r && arr[l] == arr[l + 1]) { ++lc; ++l; }
                        while (r - 1 > l && arr[r] == arr[r - 1]) { ++rc; --r; }
                        ans = (ans + (lc * rc) % MOD) % MOD;
                        ++l; --r;
                    }
                }
            }
        }
        return (int)(ans % MOD);
    }
};
```

### Why the `arr[l] == arr[r]` case uses `C(m,2)`
If `arr[l] == arr[r]`, every element in `[l..r]` equals the same value, so any two indices picked in this window form a valid pair with `arr[i]`. The number of unordered pairs is `m choose 2`.

---

## Longest Subarray With Sum K (Non‑negative numbers only)

When the array contains only non‑negative values (0 or >0), we can solve
[Longest Subarray With Sum K](https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1)
with a two‑pointer sliding window in O(n) time. The window expands `r` while the running sum ≤ K and
shrinks from `l` while the sum > K; whenever sum == K, update the best length.

```cpp
int longestSubarraySumK(const vector<int>& a, long long K) {
    int n = (int)a.size();
    int l = 0; long long sum = 0; int best = 0;
    for (int r = 0; r < n; ++r) {
        sum += a[r];
        while (l <= r && sum > K) {        // shrink until sum <= K
            sum -= a[l++];
        }
        if (sum == K) best = max(best, r - l + 1);
    }
    return best;
}
```

Note: This works only for non‑negative arrays. With negatives, sliding window fails; use prefix‑sum + hashmap instead.

---

## Dutch National Flag (DNF) – Single‑pass 3‑way partition

Problem (Sort Colors): Given an array containing only 0s, 1s, and 2s, sort it in-place so that all 0s come first, then 1s, then 2s. 

### Intuition
Maintain three regions using pointers `low`, `mid`, `high` and a single scan:
- `[0 .. low-1]` are 0s
- `[low .. mid-1]` are 1s
- `[mid .. high]` are unknown (to be classified)
- `[high+1 .. n-1]` are 2s

Process `nums[mid]`:
- If it is 0 → swap with `nums[low]`, advance both `low` and `mid`.
- If it is 1 → it is already in the middle region; advance `mid`.
- If it is 2 → swap with `nums[high]`, decrement `high` (do not advance `mid` because the swapped value is unclassified).

This yields O(n) time, O(1) extra space, and a single pass.

### Code (C++)
```cpp
// Sort Colors: Dutch National Flag
void sortColors(vector<int>& nums) {
    int n = (int)nums.size();
    int low = 0, mid = 0, high = n - 1;

    while (mid <= high) {
        switch (nums[mid]) {
            case 0:
                swap(nums[low++], nums[mid++]);
                break;
            case 1:
                ++mid;
                break;
            case 2:
                swap(nums[mid], nums[high--]);
                break;
        }
    }
}
```

### Practice Problems:
-  [Sort Colors](https://leetcode.com/problems/sort-colors/description/)
- [Remove Element](https://leetcode.com/problems/remove-element/)
- [Remove Duplicates From Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

- [Search In Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

- [Remove Duplicates From Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)

```CPP
int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        int j = 1;
        int count = 1;
        for (int i=1;i<n;i++){
            if (nums[i] == nums[i-1])
                count++;
            else
                count = 1;
            if (count <= 2){
                nums[j] = nums[i];
                j++;
            }
        }

        return j;
    }
```