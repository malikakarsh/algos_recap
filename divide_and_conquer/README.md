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

---

## Weighted Inversion Counting

### Problem Statement
Given two arrays of length `n`:
- `A` (values): Array of numerical values
- `W` (weights): Array of positive weights, where `W[i]` is the weight for `A[i]`

A pair `(i, j)` with `i < j` and `A[i] > A[j]` is a **weighted inversion**. Its contribution is `W[i]` (the weight of the first element).

**Goal**: Compute the total sum of weights for all such inversions:
```
Total Weighted Inversions = Σ W[i] for all (i,j) where i<j and A[i]>A[j]
```

### Test Cases

**Test Case 1**: Simple mix of weights and values
```
Index:  1   2   3   4
A:      5   2   5   1
W:      10  3   7   4

Inversions:
- (1,2): A[1]=5 > A[2]=2 → contributes W[1]=10
- (1,4): A[1]=5 > A[4]=1 → contributes W[1]=10
- (2,4): A[2]=2 > A[4]=1 → contributes W[2]=3
- (3,4): A[3]=5 > A[4]=1 → contributes W[3]=7

Expected: 10 + 10 + 3 + 7 = 30
```

**Test Case 2**: Max inversions with small weights on large numbers
```
Index:  1   2   3   4   5
A:      4   7   2   3   1
W:      10  1   5   8   2

Expected: 30 + 3 + 5 + 8 + 0 = 46
```

### Algorithm: Merge Sort Based (O(n log n))

**Intuition**: Similar to standard inversion counting, but instead of counting pairs, we sum the weights of the first element in each inversion. During merge, when `A[i] > A[j]` (left element greater than right), all remaining elements in the right half (from current `j` to `r`) form inversions with `A[i]`, contributing `W[i]` for each.

**Key insight**: When merging sorted halves, if `A[leftIdx] <= A[rightIdx]`, we take the left element. At this point, it has already been compared with all right elements processed so far (which were smaller), so we add `W[leftIdx] × countOfProcessedRightElements`.

### Code (C++)
```cpp
long long mergeWeightedInversions(vector<int>& values, vector<int>& weights,
                                   int left, int mid, int right) {
    vector<int> sortedValues, sortedWeights;
    sortedValues.reserve(right - left + 1);
    sortedWeights.reserve(right - left + 1);

    int leftIdx = left, rightIdx = mid + 1;
    long long weightedInversions = 0;
    int rightElementsProcessed = 0;  // count of right elements already merged

    while (leftIdx <= mid && rightIdx <= right) {
        if (values[leftIdx] <= values[rightIdx]) {
            // Left element goes first: it has inversions with all processed right elements
            sortedValues.push_back(values[leftIdx]);
            sortedWeights.push_back(weights[leftIdx]);
            weightedInversions += (long long)weights[leftIdx] * rightElementsProcessed;
            ++leftIdx;
        } else {
            // Right element is smaller: just merge it, increment processed count
            sortedValues.push_back(values[rightIdx]);
            sortedWeights.push_back(weights[rightIdx]);
            ++rightElementsProcessed;
            ++rightIdx;
        }
    }

    // Remaining left elements: each has inversions with all processed right elements
    while (leftIdx <= mid) {
        sortedValues.push_back(values[leftIdx]);
        sortedWeights.push_back(weights[leftIdx]);
        weightedInversions += (long long)weights[leftIdx] * rightElementsProcessed;
        ++leftIdx;
    }

    // Remaining right elements: no inversions to count
    while (rightIdx <= right) {
        sortedValues.push_back(values[rightIdx]);
        sortedWeights.push_back(weights[rightIdx]);
        ++rightIdx;
    }

    // Copy merged arrays back
    for (int i = 0; i < (int)sortedValues.size(); ++i) {
        values[left + i] = sortedValues[i];
        weights[left + i] = sortedWeights[i];
    }

    return weightedInversions;
}

long long countWeightedInversions(vector<int>& values, vector<int>& weights,
                                   int left, int right) {
    if (left >= right) return 0;

    int mid = left + (right - left) / 2;
    long long leftInversions = countWeightedInversions(values, weights, left, mid);
    long long rightInversions = countWeightedInversions(values, weights, mid + 1, right);
    long long crossInversions = mergeWeightedInversions(values, weights, left, mid, right);

    return leftInversions + rightInversions + crossInversions;
}

// Main function
long long totalWeightedInversions(vector<int>& A, vector<int>& W) {
    return countWeightedInversions(A, W, 0, (int)A.size() - 1);
}
```

### Complexity Analysis

**Recurrence**: `T(n) = 2T(n/2) + O(n)`

- **Divide**: Split into two halves → O(1)
- **Conquer**: Recursively solve left and right → `2T(n/2)`
- **Combine**: Merge step processes each element once → O(n)

**Solution**: By Master Theorem (Case 2, since `f(n) = Θ(n)` and `log_b(a) = log_2(2) = 1`):
- `T(n) = Θ(n log n)`

**Space Complexity**: O(n) for temporary arrays during merge, O(log n) for recursion stack.

**Time Complexity**: O(n log n) — optimal for comparison-based inversion counting.

