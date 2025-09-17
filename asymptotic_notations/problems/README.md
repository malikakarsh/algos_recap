# Asymptotic Analysis Problems

## Inductive Proof Method

Inductive proof is a powerful mathematical technique commonly used to prove statements about asymptotic notations, particularly when dealing with Big O, Omega, and Theta bounds.

### What is Mathematical Induction?

Mathematical induction is a method of proof used to establish that a statement P(n) is true for all natural numbers n ≥ n₀, where n₀ is some starting value.

### Structure of Inductive Proof

An inductive proof consists of two main parts:

1. **Base Case**: Prove that P(n₀) is true for the smallest value n₀
2. **Inductive Step**: Assume P(k) is true for some k ≥ n₀ (inductive hypothesis), then prove that P(k+1) is also true

### How to Solve Inductive Proofs

#### Step-by-Step Process:

1. **Identify what needs to be proven**
   - Clearly state the proposition P(n)
   - Determine the starting value n₀

2. **Prove the Base Case**
   - Substitute n₀ into P(n)
   - Show that the statement holds for this initial value
   - This is usually straightforward computation

3. **State the Inductive Hypothesis**
   - Assume P(k) is true for some arbitrary k ≥ n₀
   - Write out exactly what this assumption means

4. **Prove the Inductive Step**
   - Start with P(k+1) and manipulate it
   - Use the inductive hypothesis P(k) wherever possible
   - Apply algebraic manipulations, inequalities, or other mathematical tools
   - Show that P(k+1) follows from P(k)

5. **Conclude**
   - State that by mathematical induction, P(n) is true for all n ≥ n₀

### Example 1: Proving a Big O Bound

**Problem**: Prove that f(n) = 3n² + 2n + 1 is O(n²)

**Solution**:

**Step 1**: We need to show that ∃c > 0, ∃n₀ > 0 such that f(n) ≤ c·n² for all n ≥ n₀

**Step 2**: Let's choose c = 6 and prove by induction that 3n² + 2n + 1 ≤ 6n² for n ≥ 1

**Base Case** (n = 1):
- f(1) = 3(1)² + 2(1) + 1 = 6
- 6·(1)² = 6
- Since 6 ≤ 6, the base case holds

**Inductive Hypothesis**: 
Assume 3k² + 2k + 1 ≤ 6k² for some k ≥ 1

**Inductive Step**: 
Prove 3(k+1)² + 2(k+1) + 1 ≤ 6(k+1)²

```
3(k+1)² + 2(k+1) + 1
= 3(k² + 2k + 1) + 2k + 2 + 1
= 3k² + 6k + 3 + 2k + 2 + 1
= 3k² + 8k + 6
= (3k² + 2k + 1) + 6k + 5
≤ 6k² + 6k + 5    (by inductive hypothesis)
= 6k² + 6k + 5

We need to show this ≤ 6(k+1)² = 6k² + 12k + 6

6k² + 6k + 5 ≤ 6k² + 12k + 6
6k + 5 ≤ 12k + 6
5 ≤ 6k + 6
-1 ≤ 6k
```

Since k ≥ 1, we have 6k ≥ 6 > -1, so the inequality holds.

**Conclusion**: By mathematical induction, f(n) ≤ 6n² for all n ≥ 1, therefore f(n) = O(n²).

### Example 2: Proving a Recurrence Relation

**Problem**: Prove that if T(1) = 1 and T(n) = 2T(n/2) + n for n > 1, then T(n) = O(n log n)

**Solution**:

**Step 1**: We'll prove by strong induction that T(n) ≤ c·n log n for some constant c

**Step 2**: Choose c = 2 and prove T(n) ≤ 2n log n for n ≥ 2

**Base Case** (n = 2):
- T(2) = 2T(1) + 2 = 2(1) + 2 = 4
- 2·2·log 2 = 4 log 2 ≈ 4(0.693) ≈ 2.77
- We need T(2) ≤ 4 log 2, but 4 > 2.77

Let's adjust and use c = 3:
- 3·2·log 2 = 6 log 2 ≈ 4.16 > 4 ✓

**Inductive Hypothesis**: 
Assume T(k) ≤ 3k log k for all k < n

**Inductive Step**:
```
T(n) = 2T(n/2) + n
≤ 2·3(n/2)log(n/2) + n    (by inductive hypothesis)
= 3n log(n/2) + n
= 3n(log n - log 2) + n
= 3n log n - 3n log 2 + n
= 3n log n + n(1 - 3 log 2)
```

Since log 2 ≈ 0.693, we have 1 - 3 log 2 ≈ 1 - 2.08 = -1.08 < 0

Therefore: T(n) ≤ 3n log n

**Conclusion**: T(n) = O(n log n)

### Common Techniques in Asymptotic Inductive Proofs

1. **Choose appropriate constants**: Start with reasonable values for c and n₀
2. **Strong induction**: Often needed for recurrence relations where T(n) depends on multiple smaller values
3. **Algebraic manipulation**: Use properties of logarithms, exponentials, and polynomials
4. **Inequality techniques**: 
   - For large n, higher-order terms dominate
   - Use the fact that log n < n for all n > 1
   - 2ⁿ grows faster than any polynomial

### Tips for Success

1. **Start simple**: Try small values of c first, then adjust if needed
2. **Work backwards**: Sometimes it helps to see what you need to prove and work towards it
3. **Use known inequalities**: Keep a list of useful inequalities handy
4. **Check your arithmetic**: Small errors can invalidate the entire proof
5. **Be explicit**: Show every step clearly, especially in the inductive step

### Practice Problems - Detailed Solutions

#### Problem 1: Prove that n! = O(nⁿ)

**Goal**: Show that ∃c > 0, ∃n₀ > 0 such that n! ≤ c·nⁿ for all n ≥ n₀

**Solution**:

**Step 1**: We'll prove by induction that n! ≤ nⁿ for n ≥ 1 (choosing c = 1)

**Base Case** (n = 1):
- 1! = 1
- 1¹ = 1
- Since 1 ≤ 1, the base case holds

**Inductive Hypothesis**: 
Assume k! ≤ kᵏ for some k ≥ 1

**Inductive Step**: 
Prove (k+1)! ≤ (k+1)^(k+1)

```
(k+1)! = (k+1) · k!
≤ (k+1) · kᵏ    (by inductive hypothesis)

We need to show: (k+1) · kᵏ ≤ (k+1)^(k+1)
Dividing both sides by (k+1): kᵏ ≤ (k+1)ᵏ

Since k < k+1, we have kᵏ ≤ (k+1)ᵏ for all k ≥ 1
```

**Conclusion**: By mathematical induction, n! ≤ nⁿ for all n ≥ 1, therefore n! = O(nⁿ).

---

#### Problem 2: Prove that 2ⁿ = Ω(n²)

**Goal**: Show that ∃c > 0, ∃n₀ > 0 such that 2ⁿ ≥ c·n² for all n ≥ n₀

**Solution**:

**Step 1**: We'll prove by induction that 2ⁿ ≥ n² for n ≥ 4

**Base Case** (n = 4):
- 2⁴ = 16
- 4² = 16
- Since 16 ≥ 16, the base case holds

**Inductive Hypothesis**: 
Assume 2ᵏ ≥ k² for some k ≥ 4

**Inductive Step**: 
Prove 2^(k+1) ≥ (k+1)²

```
2^(k+1) = 2 · 2ᵏ
≥ 2 · k²    (by inductive hypothesis)
= 2k²

We need to show: 2k² ≥ (k+1)²
2k² ≥ k² + 2k + 1
k² ≥ 2k + 1
k² - 2k - 1 ≥ 0

For k ≥ 4: k² - 2k - 1 = k(k-2) - 1 ≥ 4(2) - 1 = 7 > 0
```

**Conclusion**: By mathematical induction, 2ⁿ ≥ n² for all n ≥ 4, therefore 2ⁿ = Ω(n²).

---

#### Problem 3: Prove that if T(n) = T(n-1) + n, then T(n) = Θ(n²)

**Goal**: Show both T(n) = O(n²) and T(n) = Ω(n²)

**Solution**:

**Step 1**: First, let's solve the recurrence to get an explicit formula

Given: T(n) = T(n-1) + n with some base case T(1) = c

```
T(n) = T(n-1) + n
T(n-1) = T(n-2) + (n-1)
T(n-2) = T(n-3) + (n-2)
...
T(2) = T(1) + 2

Adding all equations:
T(n) = T(1) + 2 + 3 + ... + n
T(n) = c + Σ(i=2 to n) i
T(n) = c + (Σ(i=1 to n) i) - 1
T(n) = c + n(n+1)/2 - 1
T(n) = (n² + n)/2 + c - 1
```

**Step 2**: Prove T(n) = O(n²)

We need to show T(n) ≤ c₁·n² for some c₁ > 0, n ≥ n₀

```
T(n) = (n² + n)/2 + c - 1
≤ n²/2 + n/2 + c - 1
≤ n²/2 + n²/2 + c - 1    (for n ≥ 1)
= n² + c - 1
≤ (1 + |c-1|)n²    (for n ≥ 1)
```

So T(n) = O(n²) with c₁ = 1 + |c-1|.

**Step 3**: Prove T(n) = Ω(n²)

We need to show T(n) ≥ c₂·n² for some c₂ > 0, n ≥ n₀

```
T(n) = (n² + n)/2 + c - 1
≥ n²/2    (for sufficiently large n where n/2 + c - 1 ≥ 0)
```

So T(n) = Ω(n²) with c₂ = 1/2.

**Conclusion**: Since T(n) = O(n²) and T(n) = Ω(n²), we have T(n) = Θ(n²).

---

#### Problem 4: Prove that log(n!) = O(n log n)

**Goal**: Show that ∃c > 0, ∃n₀ > 0 such that log(n!) ≤ c·n log n for all n ≥ n₀

**Solution**:

**Step 1**: We'll use the fact that log(n!) = log(1) + log(2) + ... + log(n)

**Method 1 - Direct Bound**:

```
log(n!) = log(1) + log(2) + ... + log(n)
≤ log(n) + log(n) + ... + log(n)    (n terms)
= n · log(n)
```

Therefore, log(n!) ≤ n log n, so log(n!) = O(n log n) with c = 1.

**Method 2 - Inductive Proof**:

**Base Case** (n = 1):
- log(1!) = log(1) = 0
- 1 · log(1) = 0
- Since 0 ≤ 0, the base case holds

**Inductive Hypothesis**: 
Assume log(k!) ≤ k log k for some k ≥ 1

**Inductive Step**: 
Prove log((k+1)!) ≤ (k+1) log(k+1)

```
log((k+1)!) = log((k+1) · k!)
= log(k+1) + log(k!)
≤ log(k+1) + k log k    (by inductive hypothesis)

We need to show: log(k+1) + k log k ≤ (k+1) log(k+1)
log(k+1) + k log k ≤ log(k+1) + k log(k+1)
k log k ≤ k log(k+1)
log k ≤ log(k+1)    (dividing by k > 0)
```

Since k < k+1, we have log k < log(k+1), so the inequality holds.

**Method 3 - Stirling's Approximation (Advanced)**:

Using Stirling's approximation: n! ≈ √(2πn) · (n/e)ⁿ

```
log(n!) ≈ log(√(2πn)) + n log(n/e)
= (1/2)log(2πn) + n log n - n log e
= (1/2)log(2π) + (1/2)log n + n log n - n
= O(n log n)
```

**Conclusion**: By any of these methods, log(n!) = O(n log n).

### Common Logarithm Values to Remember

When solving asymptotic analysis problems, these logarithm values are frequently useful:

#### **Base 2 Logarithms (most common in computer science)**
- log₂(1) = 0
- log₂(2) = 1
- log₂(4) = 2
- log₂(8) = 3
- log₂(16) = 4
- log₂(32) = 5
- log₂(64) = 6
- log₂(128) = 7
- log₂(256) = 8
- log₂(1024) = 10

#### **Natural Logarithms (base e)**
- ln(1) = 0
- ln(e) = 1 ≈ 1
- ln(2) ≈ 0.693
- ln(3) ≈ 1.099
- ln(4) ≈ 1.386
- ln(10) ≈ 2.303

#### **Base 10 Logarithms**
- log₁₀(1) = 0
- log₁₀(10) = 1
- log₁₀(100) = 2
- log₁₀(1000) = 3

#### **Useful Logarithm Properties**
- **Change of base**: log_a(x) = log_b(x) / log_b(a)
- **Product rule**: log(xy) = log(x) + log(y)
- **Quotient rule**: log(x/y) = log(x) - log(y)
- **Power rule**: log(x^n) = n·log(x)
- **Base conversion**: log₂(n) = ln(n) / ln(2) ≈ 1.44 · ln(n)

#### **Key Approximations for Problem Solving**
- **log₂(n) ≈ 3.32 · log₁₀(n)** (useful for converting between bases)
- **ln(n!) ≈ n ln(n) - n** (Stirling's approximation, simplified)
- **For large n**: log(n) << √n << n << n log(n) << n² << 2ⁿ

#### **Memory Tips**
1. **Powers of 2**: Remember that 2¹⁰ = 1024 ≈ 10³, so log₂(1000) ≈ 10
2. **ln(2) ≈ 0.7**: Close to 2/3, useful for quick estimates
3. **e ≈ 2.718**: Remember as "2.7 and then 1,8" 
4. **Common inequalities**: 
   - log(n) < √n for all n > 1
   - √n < n for all n > 1
   - n < 2ⁿ for all n > 0

### Additional Practice Problems - Big O Proofs

#### Problem 5: Prove that 3n² + 2n ∈ O(n² - 10n)

**Goal**: Show that ∃c > 0, ∃n₀ > 0 such that 3n² + 2n ≤ c(n² - 10n) for all n ≥ n₀

**Solution**:

**Step 1**: First, we need n² - 10n > 0, which means n(n - 10) > 0, so n > 10.

**Step 2**: We need to find c such that 3n² + 2n ≤ c(n² - 10n) for n ≥ n₀

Rearranging: 3n² + 2n ≤ cn² - 10cn
(3 - c)n² + (2 + 10c)n ≤ 0

For this to hold for large n, we need:
- 3 - c ≤ 0, so c ≥ 3
- Let's try c = 4

**Step 3**: Prove by induction that 3n² + 2n ≤ 4(n² - 10n) for n ≥ 11

**Base Case** (n = 11):
- Left side: 3(121) + 2(11) = 363 + 22 = 385
- Right side: 4(121 - 110) = 4(11) = 44
- 385 ≤ 44? **FALSE**

Let's try c = 5:
- Right side: 5(121 - 110) = 5(11) = 55
- 385 ≤ 55? **FALSE**

**Analysis**: This suggests the statement might be **FALSE**. Let's check the limit:

lim(n→∞) (3n² + 2n)/(n² - 10n) = lim(n→∞) (3 + 2/n)/(1 - 10/n) = 3/1 = 3

Since the limit exists and equals 3, we need c ≥ 3. But for small values after n = 10, the denominator n² - 10n is very small, making the ratio very large.

**Conclusion**: **3n² + 2n ∉ O(n² - 10n)** because although the leading terms have the right relationship, the negative term -10n in the denominator makes it too small for small values of n > 10.

---

#### Problem 6: Prove that 3n² + 2n ∈ O(n³ - 5n²)

**Goal**: Show that ∃c > 0, ∃n₀ > 0 such that 3n² + 2n ≤ c(n³ - 5n²) for all n ≥ n₀

**Solution**:

**Step 1**: First, we need n³ - 5n² > 0, which means n²(n - 5) > 0, so n > 5.

**Step 2**: We'll prove by induction that 3n² + 2n ≤ 1·(n³ - 5n²) for n ≥ 6

**Base Case** (n = 6):
- Left side: 3(36) + 2(6) = 108 + 12 = 120
- Right side: 6³ - 5(36) = 216 - 180 = 36
- 120 ≤ 36? **FALSE**

Let's try c = 4 and n₀ = 6:
- Right side: 4(216 - 180) = 4(36) = 144
- 120 ≤ 144? **TRUE**

**Base Case** (n = 6): ✓

**Inductive Hypothesis**: 
Assume 3k² + 2k ≤ 4(k³ - 5k²) for some k ≥ 6

**Inductive Step**: 
Prove 3(k+1)² + 2(k+1) ≤ 4((k+1)³ - 5(k+1)²)

```
Left side: 3(k+1)² + 2(k+1)
= 3(k² + 2k + 1) + 2k + 2
= 3k² + 6k + 3 + 2k + 2
= 3k² + 8k + 5

Right side: 4((k+1)³ - 5(k+1)²)
= 4(k³ + 3k² + 3k + 1 - 5k² - 10k - 5)
= 4(k³ - 2k² - 7k - 4)
= 4k³ - 8k² - 28k - 16

We need: 3k² + 8k + 5 ≤ 4k³ - 8k² - 28k - 16
Rearranging: 4k³ - 11k² - 36k - 21 ≥ 0

For k ≥ 6: 4k³ grows much faster than 11k² + 36k + 21
```

**Conclusion**: **3n² + 2n ∈ O(n³ - 5n²)** with c = 4 and n₀ = 6.

---

#### Problem 7: Prove that n¹⁰⁰ ∈ O(2ⁿ)

**Goal**: Show that ∃c > 0, ∃n₀ > 0 such that n¹⁰⁰ ≤ c·2ⁿ for all n ≥ n₀

**Solution**:

**Step 1**: We'll prove by induction that n¹⁰⁰ ≤ 2ⁿ for n ≥ n₀ (trying c = 1 first)

**Key Insight**: For any polynomial nᵏ and exponential aⁿ (where a > 1), the exponential eventually dominates.

**Finding n₀**: We need to find where 2ⁿ > n¹⁰⁰ first occurs.
- This is a very large number, let's use the fact that 2ⁿ grows exponentially
- We can show this by proving that for large enough n, (2ⁿ⁺¹)/(2ⁿ) = 2 > (n+1)¹⁰⁰/n¹⁰⁰

**Step 2**: For the inductive approach, let's prove that for sufficiently large n₀:

**Inductive Hypothesis**: 
Assume k¹⁰⁰ ≤ 2ᵏ for some k ≥ n₀

**Inductive Step**: 
Prove (k+1)¹⁰⁰ ≤ 2ᵏ⁺¹

```
We have: 2ᵏ⁺¹ = 2 · 2ᵏ ≥ 2 · k¹⁰⁰ (by inductive hypothesis)

We need: (k+1)¹⁰⁰ ≤ 2k¹⁰⁰
Dividing by k¹⁰⁰: ((k+1)/k)¹⁰⁰ ≤ 2
Taking 100th root: (k+1)/k ≤ 2^(1/100) ≈ 1.007

Since (k+1)/k = 1 + 1/k, we need: 1 + 1/k ≤ 1.007
This gives us: 1/k ≤ 0.007, so k ≥ 143
```

**Base Case**: We can verify that for n₀ = 600 (being conservative), 600¹⁰⁰ < 2⁶⁰⁰

**Conclusion**: **n¹⁰⁰ ∈ O(2ⁿ)** - exponential functions always dominate polynomial functions.

---

#### Problem 8: Prove that n³ ∉ O(10n²)

**Goal**: Show that there do NOT exist c > 0, n₀ > 0 such that n³ ≤ c·10n² for all n ≥ n₀

**Solution**:

**Step 1**: Assume for contradiction that n³ ∈ O(10n²)

This means ∃c > 0, ∃n₀ > 0 such that n³ ≤ 10c·n² for all n ≥ n₀

**Step 2**: Simplify the inequality
n³ ≤ 10c·n²
Dividing both sides by n² (since n > 0): n ≤ 10c

**Step 3**: This means n is bounded by the constant 10c for all n ≥ n₀

But this is impossible since n can be arbitrarily large.

**Alternative Proof by Contradiction using Limits**:

```
If n³ ∈ O(10n²), then lim(n→∞) n³/(10n²) should be finite.

lim(n→∞) n³/(10n²) = lim(n→∞) n/10 = ∞
```

Since this limit is infinite, no constant c can bound the ratio.

**Inductive Disproof**: 

Suppose n³ ≤ c·10n² for all n ≥ n₀. 

For any proposed c and n₀, choose n₁ = max(n₀, 10c + 1).

Then: n₁³ = (10c + 1)³ > (10c)³ = 1000c³
And: c·10n₁² = c·10(10c + 1)² = 10c(100c² + 20c + 1) = 1000c³ + 200c² + 10c

For large c: n₁³ > c·10n₁², contradicting our assumption.

**Conclusion**: **n³ ∉ O(10n²)** because cubic functions grow faster than quadratic functions, regardless of constants.

### Theoretical Problems - Fundamental Theorems

#### Problem 9: Prove or Disprove: f(n) = O(g(n)) ⇔ g(n) = Ω(f(n))

**Answer**: **TRUE** - This is a fundamental theorem in asymptotic analysis.

**Proof**:

**Direction 1**: f(n) = O(g(n)) ⇒ g(n) = Ω(f(n))

Given: f(n) = O(g(n))
This means: ∃c > 0, ∃n₀ > 0 such that f(n) ≤ c·g(n) for all n ≥ n₀

To prove: g(n) = Ω(f(n))
We need: ∃c' > 0, ∃n₁ > 0 such that g(n) ≥ c'·f(n) for all n ≥ n₁

From f(n) ≤ c·g(n), we can rearrange to get:
g(n) ≥ (1/c)·f(n) for all n ≥ n₀

Choose c' = 1/c and n₁ = n₀. Then g(n) ≥ c'·f(n) for all n ≥ n₁. ∎

**Direction 2**: g(n) = Ω(f(n)) ⇒ f(n) = O(g(n))

Given: g(n) = Ω(f(n))
This means: ∃c' > 0, ∃n₁ > 0 such that g(n) ≥ c'·f(n) for all n ≥ n₁

To prove: f(n) = O(g(n))
We need: ∃c > 0, ∃n₀ > 0 such that f(n) ≤ c·g(n) for all n ≥ n₀

From g(n) ≥ c'·f(n), we can rearrange to get:
f(n) ≤ (1/c')·g(n) for all n ≥ n₁

Choose c = 1/c' and n₀ = n₁. Then f(n) ≤ c·g(n) for all n ≥ n₀. ∎

**Conclusion**: The statement is **TRUE**. This theorem shows the duality between Big O and Big Omega.

---

#### Problem 10: Prove or Disprove: f(n) = Θ(g(n)) ⇔ f(n) = O(g(n)) and f(n) = Ω(g(n))

**Answer**: **TRUE** - This is the definition of Big Theta.

**Proof**:

**Direction 1**: f(n) = Θ(g(n)) ⇒ f(n) = O(g(n)) and f(n) = Ω(g(n))

Given: f(n) = Θ(g(n))
This means: ∃c₁, c₂ > 0, ∃n₀ > 0 such that c₁·g(n) ≤ f(n) ≤ c₂·g(n) for all n ≥ n₀

**To prove f(n) = O(g(n))**:
From the upper bound f(n) ≤ c₂·g(n), we directly have f(n) = O(g(n)) with constant c₂ and threshold n₀. ✓

**To prove f(n) = Ω(g(n))**:
From the lower bound c₁·g(n) ≤ f(n), we directly have f(n) = Ω(g(n)) with constant c₁ and threshold n₀. ✓

**Direction 2**: f(n) = O(g(n)) and f(n) = Ω(g(n)) ⇒ f(n) = Θ(g(n))

Given:
- f(n) = O(g(n)): ∃c₂ > 0, ∃n₁ > 0 such that f(n) ≤ c₂·g(n) for all n ≥ n₁
- f(n) = Ω(g(n)): ∃c₁ > 0, ∃n₂ > 0 such that f(n) ≥ c₁·g(n) for all n ≥ n₂

**To prove f(n) = Θ(g(n))**:
Choose n₀ = max(n₁, n₂). Then for all n ≥ n₀:
- f(n) ≤ c₂·g(n) (from the Big O condition)
- f(n) ≥ c₁·g(n) (from the Big Omega condition)

Therefore: c₁·g(n) ≤ f(n) ≤ c₂·g(n) for all n ≥ n₀

This is exactly the definition of f(n) = Θ(g(n)) with constants c₁, c₂ and threshold n₀. ∎

**Conclusion**: The statement is **TRUE**. This theorem shows that Theta is the intersection of Big O and Big Omega.

---

#### Problem 11: Prove or Disprove: f(n) = O(g(n)) or f(n) = Ω(g(n))

**Answer**: **FALSE** - This statement is not always true.

**Counterexample**:

Let f(n) = n^(1 + sin(ln(ln(n)))) and g(n) = n.

The function sin(ln(ln(n))) oscillates between -1 and 1, so f(n) oscillates between n^0 = 1 and n^2.

**Analysis**:
- **f(n) ≠ O(g(n))**: Since f(n) can be as large as n², we cannot bound f(n) ≤ c·n for any constant c
- **f(n) ≠ Ω(g(n))**: Since f(n) can be as small as 1, we cannot bound f(n) ≥ c·n for any positive constant c

**Simpler Counterexample 1**:

Let f(n) = n^(1 + sin(n)) and g(n) = n.

- When sin(n) ≈ 1: f(n) ≈ n², so f(n) grows much faster than g(n)
- When sin(n) ≈ -1: f(n) ≈ n⁰ = 1, so f(n) grows much slower than g(n)
- Since sin(n) oscillates infinitely often, neither O nor Ω relationship holds

**Even Simpler Counterexample 2**:

Let f(n) = n² and g(n) = {1 if n is odd, n³ if n is even}

**Analysis**:

**Testing f(n) = O(g(n))**:
We need ∃c > 0, ∃n₀ > 0 such that n² ≤ c·g(n) for all n ≥ n₀

- **For odd n**: n² ≤ c·1 = c
  This means n² ≤ c for all odd n ≥ n₀
  But n² can be arbitrarily large for odd values, so no constant c works
- **Conclusion**: f(n) ≠ O(g(n)) ✗

**Testing f(n) = Ω(g(n))**:
We need ∃c > 0, ∃n₀ > 0 such that n² ≥ c·g(n) for all n ≥ n₀

- **For even n**: n² ≥ c·n³
  This simplifies to: 1 ≥ c·n for all even n ≥ n₀
  As n grows, c·n can be arbitrarily large, so this is impossible for any c > 0
- **Conclusion**: f(n) ≠ Ω(g(n)) ✗

**Why This Example Works**:

1. **g(n) alternates between very small (1) and very large (n³) values**
2. **For odd n**: g(n) = 1 is too small to provide an upper bound for n²
3. **For even n**: g(n) = n³ is too large to allow n² as a lower bound
4. **The alternating behavior prevents both O and Ω relationships**

**Visual Understanding**:
```
n:     1   2   3   4   5   6   7   8   9   10  ...
f(n):  1   4   9  16  25  36  49  64  81  100  ...
g(n):  1   8   1  64   1 216   1 512   1 1000  ...
```

Notice how f(n) = n² is sometimes much larger than g(n) (when n is odd) and sometimes much smaller (when n is even, for large n).

**Why the Original "Proof" Was Wrong**:

The error was assuming that if lim(n→∞) f(n)/g(n) doesn't exist, then we can still establish one of the relationships. This is false when the ratio oscillates without bound in both directions.

**Correct Statement**:

The statement would be true if we restrict to functions where lim(n→∞) f(n)/g(n) exists (including ∞). But for general functions, especially those with oscillatory behavior, neither relationship may hold.

**When IS the Statement True?**

The statement f(n) = O(g(n)) or f(n) = Ω(g(n)) is true when:
1. Both functions are "well-behaved" (monotonic or eventually monotonic)
2. The limit lim(n→∞) f(n)/g(n) exists (including 0 or ∞)
3. We restrict to common algorithm analysis contexts where pathological oscillations don't occur

**Examples**:
1. **f(n) = n, g(n) = n²**: f(n) = O(g(n)) ✓ (limit = 0)
2. **f(n) = n², g(n) = n**: f(n) = Ω(g(n)) ✓ (limit = ∞)  
3. **f(n) = n^(1+sin(n)), g(n) = n**: Neither O nor Ω ✗ (no limit)

**Conclusion**: The statement is **FALSE** in general, though it holds for most functions encountered in typical algorithm analysis.

---

### Function Comparison Table Problem

#### Problem 12: For each pair of functions f, g in the following table, indicate whether f is O, Ω, or Θ of g.

| f(n) | g(n) | O | Ω | Θ |
|------|------|---|---|---|
| 3n - 50 | n² - 7n | ? | ? | ? |
| n² + 100n | 2n² - n | ? | ? | ? |
| log n | √n | ? | ? | ? |
| n! | 2ⁿ | ? | ? | ? |
| n log n | n^1.5 | ? | ? | ? |
| 3^n | 2^n | ? | ? | ? |
| n³ | n² log n | ? | ? | ? |
| 100 | log n | ? | ? | ? |
| log₂ n | log₁₀ n | ? | ? | ? |

**Solutions**:

| f(n) | g(n) | O | Ω | Θ | Explanation |
|------|------|---|---|---|-------------|
| **3n - 50** | **n² - 7n** | **✓** | **✗** | **✗** | Linear vs quadratic: lim(n→∞) (3n-50)/(n²-7n) = lim(n→∞) (3-50/n)/(n-7) = 0 |
| **n² + 100n** | **2n² - n** | **✓** | **✓** | **✓** | Both quadratic: lim(n→∞) (n²+100n)/(2n²-n) = lim(n→∞) (1+100/n)/(2-1/n) = 1/2 |
| **log n** | **√n** | **✓** | **✗** | **✗** | Log vs polynomial: lim(n→∞) (log n)/√n = 0 (L'Hôpital's rule) |
| **n!** | **2ⁿ** | **✗** | **✓** | **✗** | Factorial grows faster: Stirling's approximation shows n! ≈ √(2πn)(n/e)ⁿ |
| **n log n** | **n^1.5** | **✓** | **✗** | **✗** | n log n vs n^1.5: lim(n→∞) (n log n)/n^1.5 = lim(n→∞) (log n)/√n = 0 |
| **3^n** | **2^n** | **✗** | **✓** | **✗** | Different exponential bases: lim(n→∞) 3ⁿ/2ⁿ = lim(n→∞) (3/2)ⁿ = ∞ |
| **n³** | **n² log n** | **✗** | **✓** | **✗** | Cubic vs quadratic-log: lim(n→∞) n³/(n² log n) = lim(n→∞) n/log n = ∞ |
| **100** | **log n** | **✗** | **✓** | **✗** | Constant vs growing: 100 ≥ c·log n fails for large n, but log n ≤ c·100 works |
| **log₂ n** | **log₁₀ n** | **✓** | **✓** | **✓** | Different log bases: log₂ n = (log₁₀ n)/(log₁₀ 2) ≈ 3.32 · log₁₀ n |

### Detailed Analysis Using Induction:

#### **Row 1: f(n) = 3n - 50, g(n) = n² - 7n**

**Testing O**: Need to show ∃c > 0, ∃n₀ > 0 such that 3n - 50 ≤ c(n² - 7n) for all n ≥ n₀

**Finding the Right n₀**: Let's first understand why we can't start from n = 1

**Important Concept: Asymptotically Positive Functions**

In asymptotic analysis, we require **BOTH f(n) and g(n) to be asymptotically positive**. This means:
- ∃n₁ > 0 such that f(n) > 0 for all n ≥ n₁
- ∃n₂ > 0 such that g(n) > 0 for all n ≥ n₂

**Checking f(n) = 3n - 50**:
f(n) > 0 when 3n - 50 > 0, so 3n > 50, which gives n > 50/3 ≈ 16.67
Therefore, f(n) is asymptotically positive for n ≥ 17.

**Checking g(n) = n² - 7n**:
g(n) > 0 when n² - 7n > 0, so n(n-7) > 0
This holds when n < 0 or n > 7
Since we need positive n, g(n) is asymptotically positive for n ≥ 8.

**Choosing n₀**: For Big O analysis, we need **both functions to be positive**, so we must choose:
n₀ ≥ max(17, 8) = 17

**Testing our choice**:

**Testing n = 17**:
- f(17) = 3(17) - 50 = 51 - 50 = 1 > 0 ✓
- g(17) = 17² - 7(17) = 289 - 119 = 170 > 0 ✓
- Is f(17) ≤ g(17)? Is 1 ≤ 170? **YES!** ✓

**Why n ≥ 17 is the correct choice**:
1. **f(n) > 0** requires n ≥ 17 (since 3n - 50 > 0 when n > 16.67)
2. **g(n) > 0** requires n ≥ 8 (since n² - 7n > 0 when n > 7)  
3. **Both positive** requires n ≥ max(17, 8) = 17
4. **The inequality holds** at n = 17

**Important Note**: 
In my earlier calculation, I incorrectly used n ≥ 8 because I only checked that g(n) > 0, but I forgot that **f(n) must also be positive**. The function f(n) = 3n - 50 is actually **negative** for n < 17, which violates the asymptotically positive requirement.

This is why **both functions matter** - we can't have negative values in our asymptotic analysis.

**Inductive Proof**: Let's prove 3n - 50 ≤ 1·(n² - 7n) for n ≥ 17

**Base Case** (n = 17):
- Left side: f(17) = 3(17) - 50 = 51 - 50 = 1
- Right side: g(17) = 17² - 7(17) = 289 - 119 = 170  
- Since 1 ≤ 170, base case holds ✓

**Inductive Hypothesis**: Assume 3k - 50 ≤ k² - 7k for some k ≥ 17

**Inductive Step**: Prove 3(k+1) - 50 ≤ (k+1)² - 7(k+1)
```
Left side: 3(k+1) - 50 = 3k + 3 - 50 = (3k - 50) + 3
Right side: (k+1)² - 7(k+1) = k² + 2k + 1 - 7k - 7 = k² - 5k - 6

We need to show: (3k - 50) + 3 ≤ k² - 5k - 6
By inductive hypothesis: 3k - 50 ≤ k² - 7k
So we need: k² - 7k + 3 ≤ k² - 5k - 6
Simplifying: -7k + 3 ≤ -5k - 6
-2k ≤ -9
k ≥ 4.5
```
Since k ≥ 8, this holds. Therefore f(n) = O(g(n)) ✓

**Testing Ω**: Need to show ∃c > 0, ∃n₀ > 0 such that 3n - 50 ≥ c(n² - 7n) for all n ≥ n₀

**Proof by Contradiction**: Suppose 3n - 50 ≥ c(n² - 7n) for some c > 0 and all n ≥ n₀
This gives us: 3n - 50 ≥ cn² - 7cn
Rearranging: cn² - (3 + 7c)n + 50 ≤ 0

For large n, the left side is dominated by cn² > 0, making the inequality impossible.
Therefore f(n) ≠ Ω(g(n)) ✗

**Testing Θ**: Since f(n) ≠ Ω(g(n)), we have f(n) ≠ Θ(g(n)) ✗

#### **Row 2: f(n) = n² + 100n, g(n) = 2n² - n**

**Testing O**: Need n² + 100n ≤ c(2n² - n) for large n

**Inductive Proof**: Let's prove n² + 100n ≤ 2(2n² - n) for n ≥ 51

**Base Case** (n = 51):
- Left side: 51² + 100(51) = 2601 + 5100 = 7701
- Right side: 2(2·51² - 51) = 2(5202 - 51) = 2(5151) = 10302
- Since 7701 ≤ 10302, base case holds ✓

**Inductive Hypothesis**: Assume k² + 100k ≤ 2(2k² - k) = 4k² - 2k for some k ≥ 51

**Inductive Step**: Prove (k+1)² + 100(k+1) ≤ 4(k+1)² - 2(k+1)
```
Left side: (k+1)² + 100(k+1) = k² + 2k + 1 + 100k + 100 = k² + 102k + 101
Right side: 4(k+1)² - 2(k+1) = 4k² + 8k + 4 - 2k - 2 = 4k² + 6k + 2

We need: k² + 102k + 101 ≤ 4k² + 6k + 2
Rearranging: 3k² - 96k - 99 ≥ 0
3(k² - 32k - 33) ≥ 0
k² - 32k - 33 ≥ 0

Using quadratic formula: k ≥ (32 + √(1024 + 132))/2 ≈ 33.7
Since k ≥ 51, this holds.
```
Therefore f(n) = O(g(n)) ✓

**Testing Ω**: Need n² + 100n ≥ c(2n² - n) for large n

**Inductive Proof**: Let's prove n² + 100n ≥ (1/4)(2n² - n) for n ≥ 1

**Base Case** (n = 1):
- Left side: 1 + 100 = 101
- Right side: (1/4)(2 - 1) = 1/4
- Since 101 ≥ 1/4, base case holds ✓

**Inductive Hypothesis**: Assume k² + 100k ≥ (1/4)(2k² - k) for some k ≥ 1

**Inductive Step**: Prove (k+1)² + 100(k+1) ≥ (1/4)(2(k+1)² - (k+1))
```
Left side: k² + 2k + 1 + 100k + 100 = k² + 102k + 101
Right side: (1/4)(2k² + 4k + 2 - k - 1) = (1/4)(2k² + 3k + 1)

We need: k² + 102k + 101 ≥ (1/4)(2k² + 3k + 1)
Multiplying by 4: 4k² + 408k + 404 ≥ 2k² + 3k + 1
Simplifying: 2k² + 405k + 403 ≥ 0

Since all coefficients are positive and k ≥ 1, this always holds.
```
Therefore f(n) = Ω(g(n)) ✓

**Testing Θ**: Since both O and Ω hold, f(n) = Θ(g(n)) ✓

#### **Row 4: f(n) = n!, g(n) = 2ⁿ**

**Testing O**: Need to show n! ≤ c·2ⁿ for some c and large n

**Proof by Contradiction**: Suppose n! ≤ c·2ⁿ for all n ≥ n₀
Consider n = 2c (assuming c ≥ n₀):
```
(2c)! ≤ c·2^(2c)
But (2c)! contains the factor (2c)(2c-1)...(c+1) ≥ c^c
And c^c grows faster than any exponential in c
```
This leads to a contradiction for sufficiently large c.
Therefore n! ≠ O(2ⁿ) ✗

**Testing Ω**: Need to show n! ≥ c·2ⁿ for some c and large n

**Inductive Proof**: Let's prove n! ≥ 1·2ⁿ for n ≥ 4

**Base Case** (n = 4):
- Left side: 4! = 24
- Right side: 2⁴ = 16
- Since 24 ≥ 16, base case holds ✓

**Inductive Hypothesis**: Assume k! ≥ 2ᵏ for some k ≥ 4

**Inductive Step**: Prove (k+1)! ≥ 2^(k+1)
```
(k+1)! = (k+1)·k! ≥ (k+1)·2ᵏ (by inductive hypothesis)

We need: (k+1)·2ᵏ ≥ 2^(k+1) = 2·2ᵏ
Dividing by 2ᵏ: k+1 ≥ 2
This holds for all k ≥ 1, certainly for k ≥ 4
```
Therefore n! = Ω(2ⁿ) ✓

**Testing Θ**: Since O fails but Ω holds, n! ≠ Θ(2ⁿ) ✗

---

#### **Row 9: f(n) = log₂ n, g(n) = log₁₀ n**

**Key Insight**: Different logarithm bases are related by a constant factor.

**Change of Base Formula**: log₂ n = (log₁₀ n)/(log₁₀ 2)

Since log₁₀ 2 ≈ 0.301, we have: log₂ n ≈ (1/0.301) · log₁₀ n ≈ 3.32 · log₁₀ n

**Testing O**: Need to show ∃c > 0, ∃n₀ > 0 such that log₂ n ≤ c · log₁₀ n for all n ≥ n₀

**Inductive Proof**: Let's prove log₂ n ≤ 4 · log₁₀ n for n ≥ 2

**Base Case** (n = 2):
- Left side: log₂ 2 = 1
- Right side: 4 · log₁₀ 2 = 4 · 0.301 ≈ 1.204
- Since 1 ≤ 1.204, base case holds ✓

**Inductive Hypothesis**: Assume log₂ k ≤ 4 · log₁₀ k for some k ≥ 2

**Inductive Step**: Prove log₂(k+1) ≤ 4 · log₁₀(k+1)

Using the change of base formula:
```
log₂(k+1) = (log₁₀(k+1))/(log₁₀ 2) = (log₁₀(k+1))/0.301 ≈ 3.32 · log₁₀(k+1)

Since 3.32 < 4, we have: log₂(k+1) < 4 · log₁₀(k+1)
```
Therefore log₂ n = O(log₁₀ n) ✓

**Testing Ω**: Need to show ∃c > 0, ∃n₀ > 0 such that log₂ n ≥ c · log₁₀ n for all n ≥ n₀

**Inductive Proof**: Let's prove log₂ n ≥ 3 · log₁₀ n for n ≥ 2

**Base Case** (n = 2):
- Left side: log₂ 2 = 1
- Right side: 3 · log₁₀ 2 = 3 · 0.301 ≈ 0.903
- Since 1 ≥ 0.903, base case holds ✓

**Inductive Hypothesis**: Assume log₂ k ≥ 3 · log₁₀ k for some k ≥ 2

**Inductive Step**: Prove log₂(k+1) ≥ 3 · log₁₀(k+1)

Using the change of base formula:
```
log₂(k+1) = (log₁₀(k+1))/(log₁₀ 2) ≈ 3.32 · log₁₀(k+1)

Since 3.32 > 3, we have: log₂(k+1) > 3 · log₁₀(k+1)
```
Therefore log₂ n = Ω(log₁₀ n) ✓

**Testing Θ**: Since both O and Ω hold, log₂ n = Θ(log₁₀ n) ✓

**Important Principle**: **All logarithms with constant bases are asymptotically equivalent**. The difference between log₂ n, log₁₀ n, ln n, etc., is only a constant factor, so they all belong to the same asymptotic complexity class.

### Summary

All four problems demonstrate different techniques in asymptotic analysis:
1. **Direct comparison** for factorial bounds
2. **Exponential vs polynomial** growth rates  
3. **Recurrence relation solving** and tight bounds
4. **Logarithmic properties** and summation bounds
5. **Big O relationships** with negative terms and proof by contradiction
6. **Fundamental theorems** about asymptotic notation relationships
