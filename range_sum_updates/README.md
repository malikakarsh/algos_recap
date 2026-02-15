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



---

## Approach 3: Segment Tree (O(log n) update, O(log n) query)
A Segment Tree allows for both efficient range updates and range queries. It is a binary tree where each node represents an interval.

**Key Operations:**
- **Build**: Construct the tree from the array. $O(n)$
- **Update**: Update a value at an index (or a range, with lazy propagation). $O(\log n)$
- **Query**: Query an aggregate (sum, max, etc.) over a range. $O(\log n)$

### Code Template (Max Segment Tree)

```cpp
class SGTree {
    int n;
    vector<int> seg;

public:
    SGTree(int n_) {
        n = n_;
        seg.resize(4 * n + 1);
    }

    void build(vector<int>& nums, int idx, int low, int high) {
        if (low == high) {
            seg[idx] = nums[low];
            return;
        }
        int mid = low + (high - low) / 2;
        build(nums, 2 * idx + 1, low, mid);
        build(nums, 2 * idx + 2, mid + 1, high);
        seg[idx] = max(seg[2 * idx + 1], seg[2 * idx + 2]);
    }

    void update(int idx, int low, int high, int i, int val) {
        if (low == high) {
            seg[idx] = val;
            return;
        }
        int mid = low + (high - low) / 2;
        if (i <= mid) update(2 * idx + 1, low, mid, i, val);
        else update(2 * idx + 2, mid + 1, high, i, val);
        seg[idx] = max(seg[2 * idx + 1], seg[2 * idx + 2]);
    }

    int query(int idx, int low, int high, int l, int r) {
        // No overlap
        // [l, r] ... [low, high] OR [low, high] ... [l, r]
        if (high < l || low > r) return INT_MIN;
        
        // Complete overlap
        // [l  ...  low ... high ... r]
        if (low >= l && high <= r) return seg[idx];

        // Partial overlap
        int mid = low + (high - low) / 2;
        int left = query(2 * idx + 1, low, mid, l, r);
        int right = query(2 * idx + 2, mid + 1, high, l, r);
        return max(left, right);
    }
};
```

**Time Complexity**: $O(\log n)$ per update and query.
**Space Complexity**: $O(n)$ (specifically $4n$) for the tree.


### Practice Problems:
- [Apply Operations to Make All Array Elements Equal to Zero](https://leetcode.com/problems/apply-operations-to-make-all-array-elements-equal-to-zero/description/)
- [Increment Submatrices By One](https://leetcode.com/problems/increment-submatrices-by-one/)
- [Corporate Flight Bookings](https://leetcode.com/problems/corporate-flight-bookings/)
- [Range Addition II](https://leetcode.com/problems/range-addition-ii/description/)
- [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
- [Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-mutable/)
- [Xenia and Bit Operations](https://codeforces.com/problemset/problem/339/D)
- [Sereja and Brackets](https://codeforces.com/problemset/problem/380/C)
- [Maximum Side Length of a Square With Sum Less than or Equal to Threshold](https://leetcode.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/)
