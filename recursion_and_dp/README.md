# Recursion and Dynamic Programming

## What is Recursion?

**Recursion** is a programming technique where a function calls itself directly or indirectly to solve a problem. It's based on the principle of **divide and conquer** - breaking down a complex problem into smaller, similar subproblems.

### Basic Structure
```cpp
returnType functionName(parameters) {
    // Base case(s) - stopping condition
    if (baseCondition) {
        return baseValue;
    }
    
    // Recursive case - function calls itself
    return functionName(modifiedParameters);
}
```

## Components of Recursion

### 1. **Base Case**
- **Purpose**: Prevents infinite recursion
- **Condition**: When the function should stop calling itself
- **Return**: Usually a simple, known value

### 2. **Recursive Case**
- **Purpose**: Breaks down the problem into smaller subproblems
- **Action**: Function calls itself with modified parameters
- **Progress**: Each call should move closer to the base case

### 3. **Recursive Call**
- **Modification**: Parameters should change to approach base case
- **Return**: Usually combines result with current computation

## Classic Examples

### 1. Factorial
```cpp
int factorial(int n) {
    // Base case
    if (n <= 1) {
        return 1;
    }
    
    // Recursive case
    return n * factorial(n - 1);
}
```

**How it works**:
```
factorial(4)
├── 4 * factorial(3)
│   ├── 3 * factorial(2)
│   │   ├── 2 * factorial(1)
│   │   │   └── return 1
│   │   └── return 2 * 1 = 2
│   └── return 3 * 2 = 6
└── return 4 * 6 = 24
```

### 2. Fibonacci Sequence
```cpp
int fibonacci(int n) {
    // Base cases
    if (n <= 1) {
        return n;
    }
    
    // Recursive case
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### 3. Binary Search (Recursive)
```cpp
int binarySearch(vector<int>& arr, int target, int left, int right) {
    // Base case
    if (left > right) {
        return -1;  // Not found
    }
    
    int mid = left + (right - left) / 2;
    
    // Base case - found
    if (arr[mid] == target) {
        return mid;
    }
    
    // Recursive cases
    if (arr[mid] > target) {
        return binarySearch(arr, target, left, mid - 1);
    } else {
        return binarySearch(arr, target, mid + 1, right);
    }
}
```

## Time Complexity Analysis

### **General Formula**
For recursive functions, time complexity depends on:
1. **Number of recursive calls** per function call
2. **Size reduction** in each recursive call
3. **Work done** in each function call

### **Common Patterns**

#### 1. **Linear Recursion** - O(n)
```cpp
int sum(int n) {
    if (n <= 0) return 0;           // Base case: O(1)
    return n + sum(n - 1);           // Recursive call: T(n-1)
}
// T(n) = T(n-1) + O(1) = O(n)
```

#### 2. **Binary Recursion** - O(2^n)
```cpp
int fibonacci(int n) {
    if (n <= 1) return n;           // Base case: O(1)
    return fibonacci(n-1) + fibonacci(n-2);  // Two recursive calls
}
// T(n) = T(n-1) + T(n-2) + O(1) ≈ O(2^n)
```

#### 3. **Divide and Conquer** - O(log n)
```cpp
int binarySearch(vector<int>& arr, int target, int left, int right) {
    if (left > right) return -1;     // Base case: O(1)
    
    int mid = left + (right - left) / 2;  // O(1)
    if (arr[mid] == target) return mid;    // O(1)
    
    if (arr[mid] > target) {
        return binarySearch(arr, target, left, mid - 1);  // T(n/2)
    } else {
        return binarySearch(arr, target, mid + 1, right); // T(n/2)
    }
}
// T(n) = T(n/2) + O(1) = O(log n)
```

#### 4. **Tree Traversal** - O(n)
```cpp
void inorderTraversal(TreeNode* root) {
    if (!root) return;               // Base case: O(1)
    
    inorderTraversal(root->left);    // T(left_subtree)
    cout << root->val << " ";        // O(1)
    inorderTraversal(root->right);   // T(right_subtree)
}
// T(n) = T(left) + T(right) + O(1) = O(n) total nodes
```

### **Master Theorem**
For divide-and-conquer algorithms: T(n) = aT(n/b) + f(n)

Where:
- **a** = number of subproblems
- **b** = size reduction factor
- **f(n)** = work done outside recursive calls

**Cases**:
1. If f(n) = O(n^c) where c < log_b(a): T(n) = O(n^log_b(a))
2. If f(n) = O(n^c) where c = log_b(a): T(n) = O(n^c log n)
3. If f(n) = O(n^c) where c > log_b(a): T(n) = O(f(n))

## Memory Storage: Call Stack

### **Where Recursion is Stored**

Recursion uses the **call stack** (also called **execution stack** or **program stack**), which is a region of memory that stores:

1. **Function parameters**
2. **Local variables**
3. **Return addresses**
4. **Temporary values**

### **Call Stack Visualization**

```cpp
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// Call: factorial(4)
```

**Stack Frames**:
```
┌─────────────────┐
│ factorial(1)    │ ← Top of stack
│ n = 1           │
│ return 1        │
├─────────────────┤
│ factorial(2)     │
│ n = 2           │
│ waiting for...  │
├─────────────────┤
│ factorial(3)     │
│ n = 3           │
│ waiting for...  │
├─────────────────┤
│ factorial(4)     │ ← Bottom of stack
│ n = 4           │
│ waiting for...  │
└─────────────────┘
```

### **Memory Characteristics**

#### **Stack Overflow**
- **Cause**: Too many recursive calls
- **Limit**: Typically 1-8 MB (varies by system)
- **Example**: Deep recursion without proper base case

```cpp
// This will cause stack overflow
int badRecursion(int n) {
    return badRecursion(n + 1);  // No base case!
}
```

#### **Memory Usage**
- **Per call**: ~8-64 bytes (depends on parameters/variables)
- **Total**: Number of calls × memory per call
- **Example**: factorial(1000) uses ~1000 stack frames

### **Stack vs Heap**

| Aspect | Call Stack | Heap |
|--------|------------|------|
| **Purpose** | Function calls, recursion | Dynamic memory allocation |
| **Size** | Limited (~1-8 MB) | Large (GBs) |
| **Management** | Automatic (LIFO) | Manual (malloc/free) |
| **Speed** | Very fast | Slower |
| **Fragmentation** | None | Possible |

## Tail Recursion

### **Definition**
Tail recursion occurs when the recursive call is the **last operation** in the function.

```cpp
// Tail recursive
int factorialTail(int n, int accumulator = 1) {
    if (n <= 1) return accumulator;
    return factorialTail(n - 1, n * accumulator);
}

// Not tail recursive
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);  // Multiplication after recursive call
}
```

### **Optimization**
Some compilers can optimize tail recursion into **iterative loops**, reducing space complexity from O(n) to O(1).

```cpp
// This can be optimized to:
int factorialIterative(int n) {
    int result = 1;
    for (int i = 1; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

## Common Recursion Patterns

### 1. **Tree Traversal**
```cpp
void preorder(TreeNode* root) {
    if (!root) return;
    cout << root->val << " ";
    preorder(root->left);
    preorder(root->right);
}
```

### 2. **Backtracking**
```cpp
void generatePermutations(vector<int>& nums, vector<int>& current, vector<bool>& used) {
    if (current.size() == nums.size()) {
        // Process permutation
        return;
    }
    
    for (int i = 0; i < nums.size(); i++) {
        if (!used[i]) {
            used[i] = true;
            current.push_back(nums[i]);
            generatePermutations(nums, current, used);
            current.pop_back();
            used[i] = false;  // Backtrack
        }
    }
}
```

#### Permutations (in-place, no extra space)
```cpp
void genPermutations(vector<int>& nums, int i, vector<vector<int>>& result) {
    if (i == (int)nums.size()) {
        result.emplace_back(nums);
        return;
    }
    for (int j = i; j < (int)nums.size(); ++j) {
        swap(nums[i], nums[j]);
        genPermutations(nums, i + 1, result);
        swap(nums[i], nums[j]);
    }
}

vector<vector<int>> permute(vector<int> nums) {
    vector<vector<int>> result;
    genPermutations(nums, 0, result);
    return result;
}
```

#### Backtracking Pattern: Start Index + Skip Duplicates (Combination Sum II)

Intuition:
- **Sort first**: Sorting lets us prune early when the current number exceeds the remaining `target`, and makes duplicate detection simple.
- **Start from an index `idx`**: Each recursive level chooses elements from positions `i >= idx`. This avoids permutations of the same combination by enforcing a left-to-right choice order.
- **Skip duplicates at the same depth**: If `candidates[i] == candidates[i-1]` and `i > idx`, skip `i` to avoid generating the same combination multiple times.
- **Prune**: Because of sorting, as soon as `candidates[i] > target`, break the loop.
- **Backtrack**: Choose → recurse with `i+1` (each number used at most once) → un-choose.

Correct and concise implementation based on your snippet (minor fixes: compare with `candidates[i]`, avoid globals, add pruning and references):

```cpp
void chooseCombination(const vector<int>& candidates,
                       int idx,
                       int target,
                       vector<int>& current,
                       vector<vector<int>>& result) {
    if (target == 0) {
        result.emplace_back(current);
        return;
    }

    for (int i = idx; i < (int)candidates.size(); ++i) {
        if (i > idx && candidates[i] == candidates[i - 1]) continue; // skip duplicates at this depth
        if (candidates[i] > target) break;                            // prune (sorted)

        current.emplace_back(candidates[i]);
        chooseCombination(candidates, i + 1, target - candidates[i], current, result);
        current.pop_back();
    }
}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    vector<vector<int>> result;
    vector<int> current;
    chooseCombination(candidates, 0, target, current, result);
    return result;
}
```

Complexity:
- Time: `O(n log n)` to sort, plus `O(n * 2^n)` in the worst case for backtracking (subset exploration with pruning). More precisely, proportional to the number of valid combinations times average combination length.
- Space: `O(n)` auxiliary for recursion depth and current path (excluding the output list of combinations).

### 3. **Divide and Conquer**
```cpp
int mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return 0;
    
    int mid = left + (right - left) / 2;
    int inversions = 0;
    
    inversions += mergeSort(arr, left, mid);
    inversions += mergeSort(arr, mid + 1, right);
    inversions += merge(arr, left, mid, right);
    
    return inversions;
}
```

## When to Use Recursion

### **Advantages**
- **Elegant code**: Often more readable than iterative solutions
- **Natural fit**: For tree/graph problems
- **Divide and conquer**: Breaks complex problems into simpler ones

### **Disadvantages**
- **Stack overflow**: Deep recursion can crash program
- **Memory overhead**: Each call uses stack space
- **Performance**: Function call overhead
- **Debugging**: Harder to trace execution

### **Best Use Cases**
- **Tree/graph traversal**
- **Mathematical sequences** (Fibonacci, factorial)
- **Backtracking problems**
- **Divide and conquer algorithms**

### **Avoid When**
- **Simple loops** would suffice
- **Deep recursion** is expected
- **Performance** is critical
- **Memory** is limited

## Debugging Recursion

### **Common Mistakes**
1. **Missing base case**: Infinite recursion
2. **Wrong base case**: Incorrect termination
3. **Not modifying parameters**: Infinite recursion
4. **Stack overflow**: Too deep recursion

### **Debugging Techniques**
```cpp
int factorial(int n, int depth = 0) {
    // Debug: Print call stack
    cout << string(depth * 2, ' ') << "factorial(" << n << ")" << endl;
    
    if (n <= 1) {
        cout << string(depth * 2, ' ') << "return 1" << endl;
        return 1;
    }
    
    int result = n * factorial(n - 1, depth + 1);
    cout << string(depth * 2, ' ') << "return " << result << endl;
    return result;
}
```

## Summary

**Recursion** is a powerful technique that:
- **Stores data** in the **call stack** (limited memory)
- **Time complexity** varies: O(n), O(log n), O(2^n), etc.
- **Space complexity** is O(depth) due to stack frames
- **Best for**: Tree problems, mathematical sequences, divide-and-conquer
- **Avoid when**: Simple iteration suffices or deep recursion expected

**Key Insight**: Recursion trades **space** (call stack) for **code simplicity** and **problem decomposition**.

### Practice Problems:
- [Combination Sum](https://leetcode.com/problems/combination-sum/)
- [Subsets](https://leetcode.com/problems/subsets/)
- [Subsets II](https://leetcode.com/problems/subsets-ii)
- [Subset Sums](https://www.geeksforgeeks.org/problems/subset-sums2234/1)
- [Permutations](https://leetcode.com/problems/permutations/)
- [Fibonacci Numbers](https://leetcode.com/problems/fibonacci-number)
- [House Robber](https://leetcode.com/problems/house-robber/)
- [House Robber II](https://leetcode.com/problems/house-robber-ii/)
- [Geeks Training](https://www.geeksforgeeks.org/problems/geeks-training/1)
- [Unique Paths](https://leetcode.com/problems/unique-paths/)
- [Maximum Sum Path In Matrix](https://www.geeksforgeeks.org/problems/path-in-matrix3805/1)
- [Chocolates Pickup](https://www.geeksforgeeks.org/problems/chocolates-pickup/1)
- [Subset Sum](https://www.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1)
- [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)
- [Perfect Sum Problem](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)
- [Partition With Given Difference](https://www.geeksforgeeks.org/problems/partitions-with-given-difference/1)
- [0-1 Knapsack](https://www.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1)
- [Coin Change](https://leetcode.com/problems/coin-change)
- [Target Sum](https://leetcode.com/problems/target-sum/)
- [Coin Change II](https://leetcode.com/problems/coin-change-ii)
- [Unbounded Knapsack](https://www.geeksforgeeks.org/problems/knapsack-with-duplicate-items4201/1)
-[Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description/)
- [Longest Common Substring](https://www.geeksforgeeks.org/problems/longest-common-substring1452/1)
- [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
- [Minimum Insertions Steps To Make A String Palindrome](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)
- [Delete Operation For Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/)
- [Shortest Common Supersequence](https://leetcode.com/problems/shortest-common-supersequence/)
- [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)
- [Edit Distance](https://leetcode.com/problems/edit-distance/)
- [Rod Cutting](https://www.geeksforgeeks.org/problems/rod-cutting0840/1)
- [Best TIme To Buy And Sell Stocks](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)
- [Best Time To Buy And Sell Stocks II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
- [Best TIme To Buy And Sell Stock With Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)
- [Best Time To Buy And Sell Stocks III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
- [Best TIme To Buy And Sell Stocks IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/)
- [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/description/)
- [Profitable Schemes](https://leetcode.com/problems/profitable-schemes/)
- [Best TIme To Buy And Sell Stock With Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)
- [Ones And Zeroes](https://leetcode.com/problems/ones-and-zeroes/)

### Longest Increasing Subsequence
```CPP
int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int>dp(n,1);
        int LIS = 1;
  
        for (int i=1;i<n;i++){
            for (int j=0;j<i;j++){
                if (nums[j] < nums[i] )
                    dp[i] = max(dp[i],1 + dp[j]);
            }
            LIS = max(LIS,dp[i]);
        }
        return LIS;
    }
```
#### Printing the LIS
```CPP
int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int>dp(n,1);
        int LIS = 1;
        vector<int>back(n);
        int ind = 0;
        back[0] = 0;
        for (int i=1;i<n;i++){
            back[i] = i;
            for (int j=0;j<i;j++){
                if (nums[j] < nums[i] && (1 + dp[j]) > dp[i]){
                    dp[i] = 1 + dp[j];
                    back[i] = j;
                }
                    
            }
            if (dp[i] > LIS){
                ind = i;
                LIS = dp[i];
            }
        }
        vector<int>res;
        while (back[ind] != ind){
            res.push_back(nums[ind]);
            ind = back[ind];
        }
        res.push_back(nums[ind]);
        reverse(res.begin(),res.end());


        for (auto num:res)
            cout<<num<<" ";
        cout<<endl;
        
        return LIS;
    }
```
- [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
- [Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset/description/)
- [Longest String Chain](https://leetcode.com/problems/longest-string-chain/description/)
- [Longest Bitonic Subsequence](https://www.geeksforgeeks.org/problems/longest-bitonic-subsequence0824/)
- [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/description/)

---

## Matrix Chain Multiplication (MCM)

### Intuition
- We have matrices A1, A2, ..., A_{n-1}. Matrix Ai has dimensions `arr[i-1] x arr[i]`. Multiplication is associative, so we can parenthesize in many ways; the cost (# of scalar multiplications) depends on the parenthesization.
- DP over intervals: let `dp[i][j]` be the minimum cost to multiply matrices `Ai ... Aj` (1-based i ≤ j).
- If we split between `k` and `k+1`, the total cost is:
  - cost of left: `dp[i][k]`
  - cost of right: `dp[k+1][j]`
  - cost to multiply the two resulting matrices: `arr[i-1] * arr[k] * arr[j]`
    - Why this term? After multiplying `Ai..Ak`, you get a matrix of size `arr[i-1] x arr[k]`. After multiplying `A{k+1}..Aj`, you get a matrix of size `arr[k] x arr[j]`. Multiplying those two costs exactly `arr[i-1] * arr[k] * arr[j]` scalar multiplications.
- Recurrence:
  - `dp[i][i] = 0`
  - `dp[i][j] = min over k in [i..j-1] of ( dp[i][k] + dp[k+1][j] + arr[i-1]*arr[k]*arr[j] )`

### Memoization (top-down)
```cpp
int mcmMemoUtil(const vector<int>& arr, int i, int j, vector<vector<int>>& dp) {
    if (i == j) return 0;
    int& memo = dp[i][j];
    if (memo != -1) return memo;
    int best = INT_MAX;
    for (int k = i; k <= j - 1; ++k) {
        long long cost = (long long)arr[i - 1] * arr[k] * arr[j]
                       + mcmMemoUtil(arr, i, k, dp)
                       + mcmMemoUtil(arr, k + 1, j, dp);
        best = min(best, (int)cost);
    }
    return memo = best;
}

int matrixMultiplicationMemo(vector<int>& arr) {
    int n = (int)arr.size();            // # matrices = n-1
    vector<vector<int>> dp(n, vector<int>(n, -1));
    return mcmMemoUtil(arr, 1, n - 1, dp);
}
```

### Tabulation (bottom-up)
```cpp
int matrixMultiplication(vector<int>& arr) {
    int n = (int)arr.size();            // matrices: A1..A_{n-1}
    vector<vector<int>> dp(n, vector<int>(n, 0));

    // Fill using i, j, k (no explicit 'len')
    for (int i = n - 1; i >= 1; --i) {
        for (int j = i + 1; j <= n - 1; ++j) {
            int best = INT_MAX;
            for (int k = i; k <= j - 1; ++k) {
                long long cost = (long long)arr[i - 1] * arr[k] * arr[j]
                               + dp[i][k] + dp[k + 1][j];
                best = min(best, (int)cost);
            }
            dp[i][j] = best;
        }
    }
    return dp[1][n - 1];
}
```

Notes:
- Both approaches are O(n^3) time and O(n^2) space, where n = arr.size().
- Use 1-based indices for matrices: Ai has dimensions arr[i-1] x arr[i].
- If you also need the optimal parenthesization, keep a `cut[i][j] = argmin k` and reconstruct.

### Practice Problems:
- [Matrix Chain Multiplication](https://www.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1)
- [Minimum Cost To Cut A Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/description/)

### **1D DP Problems**
- [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)
- [Climbing Stairs II](https://leetcode.com/problems/climbing-stairs-ii/description/)
- [House Robber](https://leetcode.com/problems/house-robber/)
- [House Robber II](https://leetcode.com/problems/house-robber-ii/)
- [House Robber III](https://leetcode.com/problems/house-robber-iii/description/)
