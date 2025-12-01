# Range Sum Updates and Point Queries

## Problem Statement
Given an array `A[0..n-1]`, support two operations:
- **Range Update**: Add a value `val` to all elements in range `[l, r]`
- **Point Query**: Return the value at index `i`

Example: Update `A[2..5]` by adding 6, then query `A[3]`.

---

## Approach 1: Naive (O(n) update, O(1) query)
Simply iterate through the range for each update.

```cpp
void rangeUpdate(vector<int>& A, int l, int r, int val) {
    for (int i = l; i <= r; ++i) A[i] += val;
}

int pointQuery(const vector<int>& A, int i) {
    return A[i];
}
```

**Time Complexity**: O(n) per update, O(1) per query  
**Space Complexity**: O(1) extra

---

## Approach 2: Difference Array (O(1) update, O(n) query)
Maintain a difference array `diff` where `diff[i] = A[i] - A[i-1]` (with `diff[0] = A[0]`).

**Key Insight**: Adding `val` to `A[l..r]` is equivalent to:
- `diff[l] += val`
- `diff[r+1] -= val` (if `r+1 < n`)

To answer a point query, compute the prefix sum of `diff` up to index `i`.

```cpp
class RangeUpdatePointQuery {
    vector<int> diff;
    int n;
public:
    RangeUpdatePointQuery(const vector<int>& A) : n((int)A.size()) {
        diff.assign(n + 1, 0);
        diff[0] = A[0];
        for (int i = 1; i < n; ++i) {
            diff[i] = A[i] - A[i - 1];
        }
    }

    void rangeUpdate(int l, int r, int val) {
        diff[l] += val;
        if (r + 1 < n) diff[r + 1] -= val;
    }

    int pointQuery(int i) {
        int sum = 0;
        for (int j = 0; j <= i; ++j) sum += diff[j];
        return sum;
    }
};
```

**Time Complexity**: O(1) per update, O(n) per query  
**Space Complexity**: O(n)


### Practice Problems:
- [Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings/)
- [Range Addition II](https://leetcode.com/problems/range-addition-ii/description/)
- [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
- [Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-mutable/)

