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