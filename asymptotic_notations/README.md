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

## What O-Notation Allows Us to Ignore

O-notation provides a powerful abstraction that allows us to ignore several implementation-specific details when analyzing algorithms:

- **Computer Architecture**: Different processors, memory hierarchies, and hardware configurations don't affect the asymptotic behavior
- **Programming Language**: Whether implemented in Python, C++, Java, or any other language, the fundamental growth rate remains the same
- **Time Measurement Units**: It doesn't matter if we measure running time in:
  - Seconds, milliseconds, or nanoseconds
  - Number of instructions executed
  - Number of basic operations performed
  
The key insight is that O-notation focuses on the **rate of growth** as input size increases, making it a universal tool for algorithm comparison regardless of these implementation details.

## Computation Model

### Random-Access Machine (RAM) Model

When analyzing algorithms, we use the **Random-Access Machine (RAM) model** as our standard computation model. This model makes several key assumptions:

- **Array Access**: Reading and writing `A[j]` takes **O(1) time**
  - We can access any element in an array in constant time
  - No matter the size of the array, accessing any index takes the same amount of time

- **Basic Operations**: Fundamental arithmetic operations take **O(1) time**:
  - Addition
  - Subtraction  
  - Multiplication
  - Comparison operations
  - Logical operations

- **Word Size**: Each integer (word) has **c log n bits**, where **c ≥ 1** is large enough
  - **Reason**: We often need to read the integer n and handle integers within the range [−n^c, n^c]
  - It is convenient to assume this takes **O(1) time**
  - This assumption allows us to treat integer operations as constant time for reasonable input sizes

### Why This Model?

The RAM model provides a simplified but realistic abstraction of modern computers that:
- Allows consistent analysis across different algorithms
- Reflects the actual performance characteristics of real computers for most practical purposes
- Makes mathematical analysis tractable while remaining practically relevant

## Asymptotically Positive Functions

**Definition**: f : ℕ → ℝ is an **asymptotically positive function** if:
∃n₀ > 0 such that ∀n > n₀ we have f(n) > 0

In other words, f(n) is positive for large enough n.

### Examples

Let's examine some functions to determine if they are asymptotically positive:

| Function | Asymptotically Positive? | Explanation |
|----------|-------------------------|-------------|
| **n² - n - 30** | **Yes** | For large n, n² dominates, so f(n) > 0 eventually |
| **2n - n²⁰** | **No** | The n²⁰ term dominates and makes f(n) negative for large n |
| **100n - n²/10 + 50** | **No** | The -n²/10 term dominates for large n, making f(n) negative |

### Important Note

**We only consider asymptotically positive functions** in asymptotic analysis. This restriction ensures that:
- Our complexity measures make practical sense (negative time/space is meaningless)
- Mathematical properties of asymptotic notations hold consistently
- Comparisons between functions remain meaningful

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

## Fundamental Theorem: Big O and Big Omega Duality

**Theorem**: f(n) = O(g(n)) ⇔ g(n) = Ω(f(n))

**English Translation**: 
- f(n) is bounded above by g(n) **if and only if** g(n) is bounded below by f(n)
- In other words: f grows no faster than g **if and only if** g grows no slower than f

### Proof:

**Direction 1**: f(n) = O(g(n)) ⇒ g(n) = Ω(f(n))

Given: f(n) = O(g(n))
This means: ∃c₁ > 0, ∃n₀ > 0 such that f(n) ≤ c₁·g(n) for all n ≥ n₀

To prove: g(n) = Ω(f(n))
We need: ∃c₂ > 0, ∃n₁ > 0 such that g(n) ≥ c₂·f(n) for all n ≥ n₁

From f(n) ≤ c₁·g(n), we get: g(n) ≥ (1/c₁)·f(n)

Choose c₂ = 1/c₁ and n₁ = n₀. Then g(n) ≥ c₂·f(n) for all n ≥ n₁. ∎

**Direction 2**: g(n) = Ω(f(n)) ⇒ f(n) = O(g(n))

Given: g(n) = Ω(f(n))
This means: ∃c₂ > 0, ∃n₁ > 0 such that g(n) ≥ c₂·f(n) for all n ≥ n₁

To prove: f(n) = O(g(n))
We need: ∃c₁ > 0, ∃n₀ > 0 such that f(n) ≤ c₁·g(n) for all n ≥ n₀

From g(n) ≥ c₂·f(n), we get: f(n) ≤ (1/c₂)·g(n)

Choose c₁ = 1/c₂ and n₀ = n₁. Then f(n) ≤ c₁·g(n) for all n ≥ n₀. ∎

### Examples:

1. **n² = O(n³)** ⇔ **n³ = Ω(n²)** ✓
   - n² grows no faster than n³, and n³ grows no slower than n²

2. **3n + 5 = O(n)** ⇔ **n = Ω(3n + 5)** ✓
   - Linear function with constants is still linear

3. **log n = O(√n)** ⇔ **√n = Ω(log n)** ✓
   - Logarithmic functions grow slower than polynomial functions

### Important Note:

This theorem is fundamental for understanding the symmetry in asymptotic analysis. It means that every upper bound statement has an equivalent lower bound statement, and vice versa.

## Fundamental Theorem: Big Theta Characterization

**Theorem**: f(n) = Θ(g(n)) if and only if f(n) = O(g(n)) and f(n) = Ω(g(n))

**English Translation**: 
- f(n) has a tight bound with g(n) **if and only if** f(n) is both upper-bounded and lower-bounded by g(n)
- In other words: f and g have the same growth rate **if and only if** f grows no faster than g AND f grows no slower than g

### Proof:

**Direction 1**: f(n) = Θ(g(n)) ⇒ f(n) = O(g(n)) and f(n) = Ω(g(n))

Given: f(n) = Θ(g(n))
This means: ∃c₁, c₂ > 0, ∃n₀ > 0 such that c₁·g(n) ≤ f(n) ≤ c₂·g(n) for all n ≥ n₀

**To prove f(n) = O(g(n))**:
From f(n) ≤ c₂·g(n), we have f(n) = O(g(n)) with constant c₂ and threshold n₀. ✓

**To prove f(n) = Ω(g(n))**:
From c₁·g(n) ≤ f(n), we have f(n) = Ω(g(n)) with constant c₁ and threshold n₀. ✓

**Direction 2**: f(n) = O(g(n)) and f(n) = Ω(g(n)) ⇒ f(n) = Θ(g(n))

Given: 
- f(n) = O(g(n)): ∃c₂ > 0, ∃n₁ > 0 such that f(n) ≤ c₂·g(n) for all n ≥ n₁
- f(n) = Ω(g(n)): ∃c₁ > 0, ∃n₂ > 0 such that f(n) ≥ c₁·g(n) for all n ≥ n₂

**To prove f(n) = Θ(g(n))**:
Choose n₀ = max(n₁, n₂). Then for all n ≥ n₀:
- f(n) ≤ c₂·g(n) (from the O condition)
- f(n) ≥ c₁·g(n) (from the Ω condition)

Therefore: c₁·g(n) ≤ f(n) ≤ c₂·g(n) for all n ≥ n₀

This is exactly the definition of f(n) = Θ(g(n)) with constants c₁, c₂ and threshold n₀. ∎

### Examples:

1. **3n² + 2n + 1 = Θ(n²)**
   - **Upper bound**: 3n² + 2n + 1 ≤ 6n² for n ≥ 1, so f(n) = O(n²)
   - **Lower bound**: 3n² + 2n + 1 ≥ 3n² for n ≥ 1, so f(n) = Ω(n²)
   - **Conclusion**: f(n) = Θ(n²) ✓

2. **n log n = Θ(n log n)**
   - **Upper bound**: n log n ≤ 1·(n log n), so f(n) = O(n log n)
   - **Lower bound**: n log n ≥ 1·(n log n), so f(n) = Ω(n log n)
   - **Conclusion**: f(n) = Θ(n log n) ✓

3. **Merge Sort running time = Θ(n log n)**
   - **Upper bound**: Merge sort never takes more than cn log n time
   - **Lower bound**: Comparison-based sorting requires at least dn log n comparisons
   - **Conclusion**: Merge sort has tight bound Θ(n log n) ✓

### Practical Implications:

1. **Algorithm Analysis**: When we prove both upper and lower bounds for an algorithm, we get a tight bound
2. **Optimality**: Θ notation tells us an algorithm is optimal within its problem class
3. **Proof Strategy**: Often easier to prove O and Ω separately, then combine for Θ
4. **Precision**: Θ gives the most precise asymptotic characterization possible

### Important Note:

This theorem shows that **Theta notation is the intersection of Big O and Big Omega**. It's the "sweet spot" where we have both upper and lower bounds, giving us the most precise asymptotic description of a function's growth rate.

## Trivial Facts on Comparison Relations

These basic facts about comparison relations form the foundation for understanding asymptotic notation relationships:

### **Fundamental Comparison Properties**

1. **Symmetry of Inequality**: 
   - **a ≤ b ⇔ b ≥ a**
   - If a is less than or equal to b, then b is greater than or equal to a

2. **Equality as Double Inequality**: 
   - **a = b ⇔ a ≤ b and a ≥ b**
   - Two values are equal if and only if each is both ≤ and ≥ the other

3. **Trichotomy Property**: 
   - **a ≤ b or a ≥ b** (or both)
   - For any two real numbers, at least one of the inequality relations must hold

### **Connection to Asymptotic Notations**

These simple comparison facts directly parallel our asymptotic notation theorems:

| Comparison Relation | Asymptotic Notation Analog |
|---------------------|----------------------------|
| **a ≤ b ⇔ b ≥ a** | **f(n) = O(g(n)) ⇔ g(n) = Ω(f(n))** |
| **a = b ⇔ a ≤ b and a ≥ b** | **f(n) = Θ(g(n)) ⇔ f(n) = O(g(n)) and f(n) = Ω(g(n))** |
| **a ≤ b or a ≥ b** | **f(n) = O(g(n)) or f(n) = Ω(g(n))** |

### **Examples with Numbers**

1. **Symmetry**: 
   - 3 ≤ 7 ⇔ 7 ≥ 3 ✓
   - Just as: n = O(n²) ⇔ n² = Ω(n) ✓

2. **Equality**: 
   - 5 = 5 ⇔ 5 ≤ 5 and 5 ≥ 5 ✓
   - Just as: n² = Θ(n²) ⇔ n² = O(n²) and n² = Ω(n²) ✓

3. **Trichotomy**: 
   - For any numbers a, b: either a ≤ b or a ≥ b (or both if a = b)
   - For functions: either f(n) = O(g(n)) or f(n) = Ω(g(n)) (or both if f(n) = Θ(g(n)))

### **Why These Facts Matter**

1. **Intuition Building**: Asymptotic notations behave like familiar comparison operators
2. **Proof Techniques**: The same logical reasoning applies to both contexts
3. **Mathematical Foundation**: These properties ensure consistency in our notation system
4. **Learning Bridge**: Connects familiar arithmetic concepts to more abstract asymptotic analysis

### **Important Note**

While these parallels are extremely helpful for intuition, remember that asymptotic notations deal with **eventual behavior** (for large n) and involve **constants and thresholds**, unlike simple numerical comparisons. However, the logical structure remains the same.

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
