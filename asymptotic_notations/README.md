# Asymptotic Notations

## What are Asymptotic Notations?

Asymptotic notations are mathematical tools used to describe the running time or space complexity of algorithms as the input size approaches infinity. They help us analyze and compare the efficiency of different algorithms by focusing on their growth rates rather than exact values.

## Algorithm Definition (Donald Knuth)

**Donald Knuth**: "An algorithm is a finite, definite, effective procedure, with some input and some output."

### Properties of an Algorithm:

**Finite**: 
- Description is finite (stronger requirement: terminate in finite number of steps)
- The algorithm must eventually stop executing

**Definite**: 
- Clearly defined, no ambiguity
- Each step must be precisely specified

**Effective**: 
- Must be realizable using a finite amount of resources
- Steps must be basic enough to be carried out

**Input**: 
- Take 0 or some inputs
- Can have zero or more input values

**Output**: 
- Produce 1 or more outputs
- Must produce at least one result

## Algorithm vs Computer Program

### Algorithm
- **Abstract**: A conceptual solution to a problem
- **Language Independent**: Can be expressed in multiple ways:
  - Natural language (English descriptions)
  - Pseudocode
  - Flowcharts
  - Mathematical notation
- **High-level**: Focuses on the logic and steps, not implementation details
- **Example**: "Sort the array by comparing adjacent elements and swapping if they're in wrong order"

### Computer Program
- **Concrete**: A specific implementation of an algorithm
- **Language Dependent**: Written in a particular programming language (Python, Java, C++, etc.)
- **Implementation Specific**: Includes syntax, data structures, and language-specific details
- **Example**: 
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
    return arr
```

### Key Relationship
- **One Algorithm → Many Programs**: A single algorithm can be implemented as multiple different computer programs
- **Algorithm First**: We design the algorithm (abstract solution) before writing the program (concrete implementation)
- **Analysis Focus**: Asymptotic analysis studies algorithms, not specific programs, because it focuses on fundamental efficiency regardless of implementation language

## Computational Problem

**Definition**: A computational problem specifies the input/output relationship.

**Algorithm Solution**: An algorithm solves a computational problem if it produces the correct output for any given input.

### Example:
```
Problem: Find the maximum element in an array
Input: Array of integers [3, 1, 4, 1, 5]
Output: Maximum value (5)

Algorithm: Linear scan through array keeping track of maximum seen so far
```

## Why Asymptotic Analysis?

- **Hardware Independence**: Focus on algorithm efficiency, not machine speed
- **Large Input Focus**: Behavior as input size grows large
- **Comparison Tool**: Compare algorithms objectively
- **Scalability**: Predict performance on larger datasets

## Types of Asymptotic Notations

### 1. Big O (O) - Upper Bound
**English Definition**: An upper bound on the running time of an algorithm. It describes the worst-case scenario and guarantees that the algorithm will not perform worse than this bound.

**Mathematical Definition**: f(n) = O(g(n)) if there exist positive constants c and n₀ such that f(n) ≤ c·g(n) for all n ≥ n₀

- **Represents**: Worst-case or upper bound
- **Meaning**: "At most" or "no worse than"
- **Usage**: Most commonly used in practice

**Example**: If an algorithm takes at most 3n² + 2n + 1 operations, we say it's O(n²)

### 2. Big Omega (Ω) - Lower Bound  
**English Definition**: A lower bound on the running time of an algorithm. It describes the best-case scenario and guarantees that the algorithm will require at least this much time/space.

**Mathematical Definition**: f(n) = Ω(g(n)) if there exist positive constants c and n₀ such that f(n) ≥ c·g(n) for all n ≥ n₀

- **Represents**: Best-case or lower bound
- **Meaning**: "At least" or "no better than"
- **Usage**: Establishes minimum time/space required

**Example**: Sorting requires Ω(n log n) comparisons in the worst case

### 3. Big Theta (Θ) - Tight Bound
**English Definition**: A tight bound on the running time of an algorithm. It is used when the Big O and Big Omega of a function are the same, indicating both a lower and upper bound on its growth.

**Mathematical Definition**: f(n) = Θ(g(n)) if f(n) = O(g(n)) and f(n) = Ω(g(n))

- **Represents**: Exact asymptotic behavior
- **Meaning**: "Exactly" or tight bound
- **Usage**: When upper and lower bounds match

**Example**: Merge sort is Θ(n log n) - always takes n log n time

### 4. Little o (o) - Strict Upper Bound
**English Definition**: A strict upper bound on the running time of an algorithm. It indicates that the function grows strictly slower than the given bound, with no possibility of equality.

**Mathematical Definition**: f(n) = o(g(n)) if for every positive constant c, there exists n₀ such that f(n) < c·g(n) for all n ≥ n₀

- **Represents**: Strictly smaller than
- **Meaning**: f(n) grows strictly slower than g(n)
- **Usage**: When we need strict inequality

**Example**: n = o(n²) because n grows strictly slower than n²

### 5. Little omega (ω) - Strict Lower Bound
**English Definition**: A strict lower bound on the running time of an algorithm. It indicates that the function grows strictly faster than the given bound, with no possibility of equality.

**Mathematical Definition**: f(n) = ω(g(n)) if for every positive constant c, there exists n₀ such that f(n) > c·g(n) for all n ≥ n₀

- **Represents**: Strictly larger than
- **Meaning**: f(n) grows strictly faster than g(n)
- **Usage**: When we need strict lower bound

**Example**: n² = ω(n) because n² grows strictly faster than n

## Visual Comparison

| Notation | Meaning | Relationship |
|----------|---------|-------------|
| f(n) = O(g(n)) | f ≤ g | Upper bound |
| f(n) = Ω(g(n)) | f ≥ g | Lower bound |
| f(n) = Θ(g(n)) | f = g | Tight bound |
| f(n) = o(g(n)) | f < g | Strict upper |
| f(n) = ω(g(n)) | f > g | Strict lower |

## Common Growth Orders (Fastest to Slowest)

1. **O(1)** - Constant time
   - Array access, hash table lookup
   
2. **O(log log n)** - Double logarithmic time
   - Some advanced data structures
   
3. **O(log n)** - Logarithmic time
   - Binary search, balanced tree operations
   
4. **O(√n)** - Square root time
   - Some number theory algorithms
   
5. **O(n)** - Linear time
   - Single loop through array, linear search
   
6. **O(n log n)** - Linearithmic time
   - Merge sort, heap sort, efficient sorting
   
7. **O(n²)** - Quadratic time
   - Bubble sort, selection sort, nested loops
   
8. **O(n³)** - Cubic time
   - Matrix multiplication, triple nested loops
   
9. **O(nᵏ)** - Polynomial time (k > 3)
   - Dynamic programming solutions
   
10. **O(2ⁿ)** - Exponential time
    - Subset generation, recursive Fibonacci
    
11. **O(n!)** - Factorial time
    - Traveling salesman (brute force), permutations
    
12. **O(nⁿ)** - Super-exponential time
    - Some recursive algorithms with poor design
    
13. **O(2^(2^n))** - Double exponential time
    - Tower of Hanoi variants, some logic problems

### Growth Rate Comparison
```
O(1) < O(log log n) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(nᵏ) < O(2ⁿ) < O(n!) < O(nⁿ) < O(2^(2^n))
```

### Practical Performance (n = 1,000,000)
- **O(1)**: 1 operation
- **O(log n)**: ~20 operations
- **O(n)**: 1,000,000 operations
- **O(n log n)**: ~20,000,000 operations
- **O(n²)**: 1,000,000,000,000 operations (impractical)
- **O(2ⁿ)**: 2^1,000,000 operations (impossible)
