# algos_recap

## const vs static const

### const
- Value cannot be changed after initialization
- Each object gets its own copy
- Must be initialized when declared

```cpp
class Example {
    const int value = 10;  // Each object has its own copy
};
```

### static const
- Shared among all objects of the class
- Only one copy exists for all instances
- Belongs to the class, not individual objects

```cpp
class Example {
    static const int MAX_SIZE = 100;  // Shared by all objects
};
```

**Key Difference**: `const` = per-object, `static const` = per-class

### static const vs inline static const

### static const
- Requires definition outside class (C++11 and earlier)
- Can cause linker errors if used in multiple translation units

```cpp
class Example {
    static const int MAX_SIZE = 100;  // Declaration
};
// Definition needed outside class:
const int Example::MAX_SIZE;  // Can cause linker issues
```

### inline static const (C++17)
- No separate definition needed
- Safe to use in headers, no linker errors

```cpp
class Example {
    inline static const int MAX_SIZE = 100;  // Complete, no external definition
};
```

**Key Difference**: `inline static const` avoids linker issues in modern C++

## emplace vs push

### push_back / push
- Takes an existing object and copies/moves it
- Object is constructed first, then copied/moved into container

```cpp
vector<string> vec;
string name = "Alice";
vec.push_back(name);        // Copy existing object
vec.push_back("Bob");       // Create temporary, then move
```

### emplace_back / emplace
- Constructs object directly in place using arguments
- More efficient - no extra copy/move operations

```cpp
vector<string> vec;
vec.emplace_back("Alice");  // Construct directly in container
vec.emplace_back(5, 'x');   // Construct string with 5 'x' chars
```

### Performance Example
```cpp
class Person {
    string name;
    int age;
public:
    Person(string n, int a) : name(n), age(a) {}
};

vector<Person> people;
people.push_back(Person("John", 25));    // Create temp + move
people.emplace_back("Jane", 30);         // Construct directly
```

**Key Difference**: `emplace` constructs in-place, `push` copies/moves existing objects

## const vs constexpr

### const
- Value cannot be changed after initialization
- Can be evaluated at runtime or compile time
- Memory may be allocated

```cpp
const int x = 10;           // Compile-time constant
const int y = rand();       // Runtime constant - OK
const string name = "John"; // Runtime initialization
```

### constexpr
- Must be evaluable at compile time
- Guarantees compile-time evaluation
- No memory allocation for true constants

```cpp
constexpr int x = 10;       // Compile-time constant
constexpr int y = x * 2;    // OK - can be computed at compile time
// constexpr int z = rand(); // ERROR - rand() not compile-time evaluable
```

### Usage Examples
```cpp
const int size1 = 100;      // Works for array size (compiler dependent)
constexpr int size2 = 100;  // Guaranteed to work for array size

int arr1[size1];            // May or may not compile
int arr2[size2];            // Always compiles
```

**Key Difference**: `const` = "won't change", `constexpr` = "computed at compile time"

### Memory Allocation Difference

**constexpr - No Memory Allocation**:
```cpp
constexpr int size = 100;
int arr[size];  // No memory allocated for 'size' variable
```
- **Compile-time constant**: Value embedded directly in machine code
- **No storage**: Treated as literal, not a variable
- **Direct substitution**: `arr[100]` instead of `arr[size]` in assembly

**const - Memory May Be Allocated**:
```cpp
const int size = 100;
int arr[size];  // May or may not work (compiler dependent)
```
- **Runtime variable**: Stored in memory as a variable
- **Memory allocated**: Takes up space in program's data section
- **Address can be taken**: `&size` is valid and points to actual memory

**Key Insight**:
```cpp
const int x = 10;
constexpr int y = 10;

int* ptr1 = &x;        // OK - x has memory address
// int* ptr2 = &y;     // ERROR - y has no memory address

int arr1[x];           // May not compile (runtime constant)
int arr2[y];           // Always compiles (compile-time constant)
```

## Vector Reserve for Efficient Allocation

### Without Reserve (Inefficient)
```cpp
vector<int> nums;
for (int i = 0; i < 1000; i++) {
    nums.push_back(i);  // Multiple reallocations as vector grows
}
// Capacity grows: 1 → 2 → 4 → 8 → 16 → 32 → ... → 1024
// Multiple memory allocations and copying
```

### With Reserve (Efficient)
```cpp
vector<int> nums;
nums.reserve(1000);     // Allocate space for 1000 elements upfront
for (int i = 0; i < 1000; i++) {
    nums.push_back(i);  // No reallocations needed
}
// Single allocation, no copying
```

### Performance Benefits
- **Prevents reallocations**: No memory copying during growth
- **Reduces time complexity**: From O(n log n) to O(n) for n insertions
- **Cache efficiency**: Contiguous memory allocation
- **Memory efficiency**: Avoids temporary extra allocations

### When to Use Reserve
```cpp
// When you know the approximate size
vector<int> adjacencyList;
adjacencyList.reserve(expectedEdges);

// Reading input with known size
int n;
cin >> n;
vector<int> data;
data.reserve(n);  // Avoid reallocations while reading
```

**Key Point**: `reserve()` only allocates memory, doesn't change `size()`. Use when you know approximate final size.

## Modern Struct with Constructor

### Traditional Struct (C-style)
```cpp
struct Node {
    int row;
    int col;
    int time;
};
// Usage: Node n = {1, 2, 3};
```

### Modern Struct with Constructor (C++)
```cpp
struct Node {
    int row, col, time;
    Node(int r, int c, int t) : row(r), col(c), time(t) {}
};
// Usage: Node n(1, 2, 3); or Node n{1, 2, 3};
```

**Benefits**:
- **Constructor**: Ensures proper initialization
- **Member initialization list**: `: row(r), col(c), time(t)` is efficient
- **Flexible creation**: Can use `Node(1, 2, 3)` or `Node{1, 2, 3}`
- **Works with containers**: `vector.emplace_back(1, 2, 3)`

## Custom Comparators for Priority Queue

### Understanding Priority Queue Behavior

**Default Priority Queue (Max Heap)**:
```cpp
priority_queue<int> pq;  // Max heap by default
pq.push(1);
pq.push(3);
pq.push(2);
cout << pq.top();  // Output: 3 (largest element)
```

### The Confusion: < vs > Operators

**Key Insight**: Priority queue uses the **opposite** of what you might expect!

#### For Max Heap (Default)
```cpp
// Uses operator< internally
// Element with HIGHEST priority comes first
priority_queue<int> maxHeap;  // Uses less<int> by default
```

#### For Min Heap
```cpp
// Uses operator> or greater<int>
// Element with LOWEST value comes first
priority_queue<int, vector<int>, greater<int>> minHeap;
```

### Why This Confusion Exists

Priority queue is implemented as a **max heap by default**. The comparator determines the **ordering**:

- **less<T>** (default): Creates max heap - largest element has highest priority
- **greater<T>**: Creates min heap - smallest element has highest priority

### Custom Comparator Examples

#### Example 1: Custom Struct with Operator Overloading

```cpp
struct Task {
    string name;
    int priority;
    int time;
    
    Task(string n, int p, int t) : name(n), priority(p), time(t) {}
    
    // For MAX heap based on priority (higher priority = higher importance)
    bool operator<(const Task& other) const {
        return priority < other.priority;  // REVERSE logic!
    }
};

// Usage
priority_queue<Task> tasks;  // Max heap - highest priority first
tasks.emplace("Email", 2, 10);
tasks.emplace("Meeting", 5, 30);
tasks.emplace("Break", 1, 15);

cout << tasks.top().name;  // Output: "Meeting" (priority = 5)
```

#### Example 2: Lambda Comparator

```cpp
// Min heap for pairs based on second element
auto cmp = [](const pair<int,int>& a, const pair<int,int>& b) {
    return a.second > b.second;  // REVERSE for min heap
};

priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
pq.push({1, 10});
pq.push({2, 5});
pq.push({3, 15});

cout << pq.top().second;  // Output: 5 (smallest second element)
```

#### Example 3: Function Object (Functor)

```cpp
struct Node {
    int val;
    int dist;
    Node(int v, int d) : val(v), dist(d) {}
};

struct CompareNode {
    bool operator()(const Node& a, const Node& b) const {
        return a.dist > b.dist;  // Min heap based on distance
    }
};

priority_queue<Node, vector<Node>, CompareNode> dijkstra;
dijkstra.emplace(1, 10);
dijkstra.emplace(2, 5);
dijkstra.emplace(3, 15);

cout << dijkstra.top().dist;  // Output: 5 (shortest distance)
```

### Memory Layout Explanation

**Why the Reverse Logic?**

Priority queue maintains a **max heap** internally. The comparator defines when element `a` should be **below** element `b` in the heap tree:

```cpp
// For max heap: larger elements should be ABOVE smaller ones
bool operator<(const T& other) const {
    return this->value < other.value;  // "I am less than other, so put other above me"
}

// For min heap: smaller elements should be ABOVE larger ones  
bool operator<(const T& other) const {
    return this->value > other.value;  // "I am greater than other, so put other above me"
}
```

### Quick Reference Guide

| Heap Type | Operator Overload | Lambda/Functor | What's on Top |
|-----------|------------------|----------------|---------------|
| **Max Heap** | `return a < b;` | `return a < b;` | **Largest** element |
| **Min Heap** | `return a > b;` | `return a > b;` | **Smallest** element |

### Why We Need Operator Overloading

#### 1. **Built-in Types Work Automatically**
```cpp
priority_queue<int> pq;  // Works - int has built-in comparison
```

#### 2. **Custom Types Need Comparison Logic**
```cpp
struct Person {
    string name;
    int age;
};

priority_queue<Person> pq;  // ERROR! No way to compare Person objects
```

#### 3. **Solutions for Custom Types**

**Option A: Operator Overloading**
```cpp
struct Person {
    string name;
    int age;
    
    bool operator<(const Person& other) const {
        return age < other.age;  // Compare by age
    }
};
```

**Option B: External Comparator**
```cpp
struct ComparePerson {
    bool operator()(const Person& a, const Person& b) const {
        return a.age < b.age;  // Max heap by age
    }
};

priority_queue<Person, vector<Person>, ComparePerson> pq;
```

### Real-World Examples

#### Dijkstra's Algorithm
```cpp
struct Edge {
    int node, dist;
    Edge(int n, int d) : node(n), dist(d) {}
    
    bool operator<(const Edge& other) const {
        return dist > other.dist;  // Min heap for shortest distance
    }
};

priority_queue<Edge> pq;  // Shortest distance first
```

#### Task Scheduling
```cpp
struct Task {
    string name;
    int deadline, duration;
    
    bool operator<(const Task& other) const {
        // Earliest deadline first (min heap behavior)
        return deadline > other.deadline;
    }
};
```

### Common Mistakes

❌ **Wrong**: Thinking `<` means "less than" for min heap
```cpp
// This creates MAX heap, not min heap!
bool operator<(const Task& other) const {
    return priority < other.priority;
}
```

✅ **Correct**: Remember the reverse logic
```cpp
// For min heap based on priority
bool operator<(const Task& other) const {
    return priority > other.priority;  // Reverse!
}
```

**Memory Tip**: "The comparator tells priority queue when to put the **other** element **above** the current one in the heap tree."