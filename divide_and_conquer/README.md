# Divide and Conquer

## Maximum Subarray Sum (Divide & Conquer)

### Intuition
- Split the array into two halves around mid `m`.
- The maximum subarray must be one of:
  1) entirely in the left half,
  2) entirely in the right half,
  3) crossing the middle — i.e., a suffix ending at `m` plus a prefix starting at `m+1`.
- Recursively solve left and right; compute the crossing sum in linear time by taking the best suffix on the left and best prefix on the right, then add them.

This yields the recurrence `T(n) = 2T(n/2) + O(n)` ⇒ `T(n) = O(n log n)` time and `O(log n)` recursion space.

```cpp
int combiningSum(const vector<int>& nums, int l, int r) {
    int m = l + (r - l) / 2;
    int leftSumMax = INT_MIN, rightSumMax = INT_MIN;

    int cur = 0;
    for (int i = m; i >= l; --i) {
        cur += nums[i];
        leftSumMax = max(leftSumMax, cur);
    }

    cur = 0;
    for (int i = m + 1; i <= r; ++i) {
        cur += nums[i];
        rightSumMax = max(rightSumMax, cur);
    }

    return leftSumMax + rightSumMax; // best crossing sum
}

int divConq(const vector<int>& nums, int l, int r) {
    if (l == r) return nums[l];
    if (l > r)  return INT_MIN;      // empty segment
    int m = l + (r - l) / 2;
    int sumLeft  = divConq(nums, l,     m);
    int sumRight = divConq(nums, m + 1, r);
    int cross    = combiningSum(nums, l, r);
    return max({sumLeft, sumRight, cross});
}

int maxSubArray(vector<int>& nums) {
    return divConq(nums, 0, (int)nums.size() - 1);
}
```

Notes:
- The crossing computation scans outward from `m` to get the maximum suffix and from `m+1` for the maximum prefix.
- For an O(n) approach, see Kadane’s algorithm in the Arrays README.

---

## Fast Exponentiation (Binary Exponentiation)

### Intuition
Exponentiation by squaring uses the identities:
- x^n = (x^{n/2})^2  if n is even
- x^n = x · (x^{(n-1)/2})^2  if n is odd

Each step halves the exponent ⇒ O(log n) multiplications. Works for integers and floating‑point bases; handle negative exponents by inverting the base.


```cpp
// Computes x^n in O(log n). Handles negative exponents.
double fastPow(double x, long long n) {
    if (n == 0) return 1.0;
    if (n < 0) { x = 1.0 / x; n = -n; }
    if (n % 2 == 0) {
        double t = fastPow(x, n / 2);
        return t * t;
    } else {
        double t = fastPow(x, (n - 1) / 2);
        return x * t * t;
    }
}
```

---

## Count Significant Inversions (merge sort counting)

Problem: Given an array `A`, count pairs `(i, j)` with `i < j` and `A[i] > 2 * A[j]`.

### Intuition
Use divide & conquer like normal inversion counting. After sorting the left and right halves, count cross pairs by two pointers: for each `i` in left, advance `j` in right while `A[i] > 2*A[j]`; the number of valid `j`s for that `i` is `j - (mid+1)`. Then merge the halves. Overall `O(n log n)`.

### Code (C++)
```cpp
long long countCross(vector<long long>& a, int l, int m, int r) {
    long long cnt = 0;
    int j = m + 1;
    for (int i = l; i <= m; ++i) {
        while (j <= r && a[i] > 2LL * a[j]) ++j;
        cnt += (j - (m + 1));
    }
    // merge step
    vector<long long> tmp; tmp.reserve(r - l + 1);
    int i = l; j = m + 1;
    while (i <= m && j <= r) {
        if (a[i] <= a[j]) tmp.push_back(a[i++]);
        else              tmp.push_back(a[j++]);
    }
    while (i <= m) tmp.push_back(a[i++]);
    while (j <= r) tmp.push_back(a[j++]);
    for (int k = 0; k < (int)tmp.size(); ++k) a[l + k] = tmp[k];
    return cnt;
}

long long sortCount(vector<long long>& a, int l, int r) {
    if (l >= r) return 0;
    int m = l + (r - l) / 2;
    long long left  = sortCount(a, l, m);
    long long right = sortCount(a, m + 1, r);
    long long cross = countCross(a, l, m, r);
    return left + right + cross;
}

long long countSignificantInversions(vector<int>& A) {
    vector<long long> a(A.begin(), A.end());
    return sortCount(a, 0, (int)a.size() - 1);
}
```



