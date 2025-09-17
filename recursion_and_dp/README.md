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
