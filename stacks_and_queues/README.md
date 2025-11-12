## Stacks and Queues (with C++)

Short recap of core ideas and minimal implementations using arrays and linked lists. In real projects, prefer the standard containers `std::stack` and `std::queue` built over `std::vector`/`std::deque`.

---

### Stack
LIFO (last-in, first-out). Operations:
- `push(x)`, `pop()`, `top()`, `empty()`, `size()` — all O(1) average.

#### Array-based Stack (dynamic array via std::vector)
```cpp
#include <bits/stdc++.h>
using namespace std;

template <typename T>
class ArrayStack {
    vector<T> data;
public:
    void push(const T& value) { data.push_back(value); }
    void pop() { if (!data.empty()) data.pop_back(); }
    T& top() { return data.back(); }
    const T& top() const { return data.back(); }
    bool empty() const { return data.empty(); }
    size_t size() const { return data.size(); }
};

int main() {
    ArrayStack<int> st;
    st.push(10); st.push(20);
    cout << st.top() << "\n"; // 20
    st.pop();
    cout << st.top() << "\n"; // 10
}
```

#### Linked-list-based Stack (push/pop at head)
```cpp
#include <bits/stdc++.h>
using namespace std;

template <typename T>
class LinkedListStack {
    struct Node { T value; Node* next; Node(const T& v, Node* n=nullptr): value(v), next(n) {} };
    Node* head = nullptr; size_t count = 0;
public:
    ~LinkedListStack() { while (head) { Node* t = head; head = head->next; delete t; } }
    void push(const T& value) { head = new Node(value, head); ++count; }
    void pop() { if (head) { Node* t = head; head = head->next; delete t; --count; } }
    T& top() { return head->value; }
    const T& top() const { return head->value; }
    bool empty() const { return head == nullptr; }
    size_t size() const { return count; }
};

int main() {
    LinkedListStack<string> st;
    st.push("a"); st.push("b");
    cout << st.top() << "\n"; // b
    st.pop();
    cout << st.top() << "\n"; // a
}
```

Trade-offs:
- Array stack: contiguous memory, great cache locality, amortized O(1) growth.
- Linked stack: O(1) push/pop without resizing; overhead per node and pointer chasing.

---

### Queue
FIFO (first-in, first-out). Operations:
- `push(x)` (enqueue), `pop()` (dequeue), `front()`, `empty()`, `size()` — all O(1) average.

#### Array-based Queue (circular buffer)
```cpp
#include <bits/stdc++.h>
using namespace std;

template <typename T>
class CircularQueue {
    vector<T> buffer; size_t head = 0, tail = 0, count = 0; size_t cap;
public:
    explicit CircularQueue(size_t capacity): buffer(capacity), cap(capacity) {}
    bool push(const T& value) { // returns false if full
        if (count == cap) return false;
        buffer[tail] = value;
        tail = (tail + 1) % cap; ++count; return true;
    }
    bool pop() { // returns false if empty
        if (count == 0) return false;
        head = (head + 1) % cap; --count; return true;
    }
    T& front() { return buffer[head]; }
    const T& front() const { return buffer[head]; }
    bool empty() const { return count == 0; }
    bool full() const { return count == cap; }
    size_t size() const { return count; }
};

int main() {
    CircularQueue<int> q(3);
    q.push(1); q.push(2); q.push(3);
    cout << q.front() << "\n"; // 1
    q.pop(); q.push(4); // wrap-around
    cout << q.front() << "\n"; // 2
}
```

#### Linked-list-based Queue (enqueue at tail, dequeue at head)
```cpp
#include <bits/stdc++.h>
using namespace std;

template <typename T>
class LinkedListQueue {
    struct Node { T value; Node* next; Node(const T& v): value(v), next(nullptr) {} };
    Node* head = nullptr; Node* tail = nullptr; size_t count = 0;
public:
    ~LinkedListQueue() { while (head) { Node* t = head; head = head->next; delete t; } }
    void push(const T& value) {
        Node* node = new Node(value);
        if (!tail) { head = tail = node; }
        else { tail->next = node; tail = node; }
        ++count;
    }
    bool pop() {
        if (!head) return false;
        Node* t = head; head = head->next; if (!head) tail = nullptr; delete t; --count; return true;
    }
    T& front() { return head->value; }
    const T& front() const { return head->value; }
    bool empty() const { return head == nullptr; }
    size_t size() const { return count; }
};

int main() {
    LinkedListQueue<int> q;
    q.push(5); q.push(6);
    cout << q.front() << "\n"; // 5
    q.pop();
    cout << q.front() << "\n"; // 6
}
```

Trade-offs:
- Circular array: fixed capacity, contiguous memory, O(1) operations.
- Linked list: dynamic growth, no fixed capacity, extra pointer overhead.

---

### Min Stack (single-stack encoding using 2*x - currentMin)
Idea: Keep one stack of encoded values and a variable `currentMin`.
- On push(x):
  - If stack empty: push x, set `currentMin = x`.
  - Else if x >= currentMin: push x (plain value).
  - Else (x < currentMin): push encoded value `2*x - currentMin`, then set `currentMin = x`.
- On pop():
  - If top >= currentMin: pop normally.
  - Else (top is encoded): the previous minimum was `prevMin = 2*currentMin - top`; set `currentMin = prevMin`, then pop.
- On top():
  - If top >= currentMin: return top.
  - Else: return `currentMin` (since top is an encoded marker for a smaller min).
- On min(): return `currentMin`.

Notes:
- Use 64-bit type for the stack storage to avoid overflow when computing `2*x - currentMin`.
- All operations are O(1) time and O(1) extra space (besides the stack itself).

```cpp
class MinStack {
    std::stack<long long> st; // stores plain or encoded values
    long long currentMin;
public:
    void push(long long x) {
        if (st.empty()) { st.push(x); currentMin = x; return; }
        if (x >= currentMin) { st.push(x); }
        else { st.push(2LL * x - currentMin); currentMin = x; }
    }
    void pop() {
        long long t = st.top(); st.pop();
        if (t < currentMin) { // encoded
            currentMin = 2LL * currentMin - t; // restore previous min
        }
    }
    long long top() const {
        long long t = st.top();
        return (t >= currentMin) ? t : currentMin;
    }
    long long min() const { return currentMin; }
};
```

Reference: Min Stack [problem link](https://leetcode.com/problems/min-stack/)

---

### Next Greater Element II (circular array, monotonic stack)
Idea: Simulate circularity by iterating indices from `2*n - 1` down to `0` and using a decreasing stack of candidate values. For index `i`, remove all values `<= nums[i % n]`. In the first pass (`i < n`), the stack top (if any) is the next greater for `i`.

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = (int)nums.size();
    vector<int> result(n, -1);
    stack<int> st; // store values in decreasing order

    for (int i = 2 * n - 1; i >= 0; --i) {
        int val = nums[i % n];
        while (!st.empty() && st.top() <= val) st.pop();
        if (i < n && !st.empty()) result[i] = st.top();
        st.push(val);
    }
    return result;
}
```

Complexity:
- Time: O(n) — each element is pushed and popped at most once.
- Space: O(n) for the stack and output.

---

### Trapping Rain Water (two pointers, O(1) space)
Idea: Maintain two pointers `l` and `r` and running maxima `lMax`, `rMax`.
- Move the pointer on the side with the smaller current height.
- If `height[l]` is less than `lMax`, it contributes `lMax - height[l]` water; else update `lMax`.
- Symmetric logic for the right side with `rMax`.

```cpp
int trap(vector<int>& height) {
    int n = (int)height.size();
    if (n == 0) return 0;
    int l = 0, r = n - 1;
    int lMax = 0, rMax = 0;
    int water = 0;
    while (l < r) {
        if (height[l] <= height[r]) {
            if (height[l] < lMax) water += lMax - height[l];
            else lMax = height[l];
            ++l;
        } else {
            if (height[r] < rMax) water += rMax - height[r];
            else rMax = height[r];
            --r;
        }
    }
    return water;
}
```

Complexity:
- Time: O(n) — each index is processed once.
- Space: O(1) extra.

---

### Shortest Unsorted Continuous Subarray (monotonic stacks)
Intuition: The left boundary is the first index that violates nondecreasing order when scanning from the left; use an increasing stack and, upon seeing a smaller next value, pop indices and track the smallest popped index as `l`. The right boundary is the last index that violates nonincreasing order when scanning from the right; use a decreasing stack and track the largest popped index as `r`. If no violation occurs, the array is already sorted.

```cpp
int findUnsortedSubarray(const vector<int>& nums) {
    int n = (int)nums.size();
    stack<int> st;
    int l = n, r = -1;

    // Left boundary: first index that must move
    for (int i = 0; i < n; ++i) {
        while (!st.empty() && nums[st.top()] > nums[i]) {
            l = min(l, st.top());
            st.pop();
        }
        st.push(i);
    }

    while (!st.empty()) st.pop();

    // Right boundary: last index that must move
    for (int i = n - 1; i >= 0; --i) {
        while (!st.empty() && nums[st.top()] < nums[i]) {
            r = max(r, st.top());
            st.pop();
        }
        st.push(i);
    }

    if (r == -1) return 0;            // already sorted
    return r - l + 1;
}
```

---

### Practice Problems
- [Shortest Unsorted Continous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)
- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/submissions/1775163391/)

- [Implement Minimum Stack](https://leetcode.com/problems/min-stack/)

- [Next Greater Element](https://leetcode.com/problems/next-greater-element-i/)

- [Next Greater Element ii](https://leetcode.com/problems/next-greater-element-ii)

- [Next Smaller Element](https://www.geeksforgeeks.org/problems/immediate-smaller-element1142/1)

- [Previous Smaller Element](https://www.geeksforgeeks.org/problems/previous-smaller-element/1)

- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)

- [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

