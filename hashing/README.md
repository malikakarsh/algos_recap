# Hashing

## What is Hashing?

**Hashing** is a technique that maps data of arbitrary size to fixed-size values (hash codes) using a hash function. The primary goal is to enable fast data retrieval, insertion, and deletion operations.

### Key Components

1. **Hash Function**: Maps keys to array indices
2. **Hash Table**: Array that stores key-value pairs
3. **Hash Code**: The result of applying hash function to a key

```cpp
// Basic hash table structure
class HashTable {
private:
    vector<pair<string, int>> table;
    int size;
    
public:
    HashTable(int s) : size(s), table(s) {}
    
    int hashFunction(string key) {
        int hash = 0;
        for (char c : key) {
            hash = (hash * 31 + c) % size;
        }
        return hash;
    }
};
```

## Hash Functions

### **Properties of Good Hash Functions**

1. **Deterministic**: Same input always produces same output
2. **Uniform Distribution**: Keys should be evenly distributed
3. **Fast Computation**: Should be O(1) or O(k) where k is key length
4. **Minimal Collisions**: Should minimize hash collisions

### **Common Hash Functions**

#### 1. **Division Method**
```cpp
int hashFunction(int key, int tableSize) {
    return key % tableSize;
}
```
- **Pros**: Simple, fast
- **Cons**: Poor distribution if table size has common factors with keys

#### 2. **Multiplication Method**
```cpp
int hashFunction(int key, int tableSize) {
    double A = 0.6180339887;  // Golden ratio - 1
    return (int)(tableSize * ((key * A) - (int)(key * A)));
}
```
- **Pros**: Better distribution than division method
- **Cons**: Slightly more complex

#### 3. **String Hashing (Polynomial Rolling Hash)**
```cpp
int hashFunction(string key, int tableSize) {
    int hash = 0;
    int base = 31;  // Prime number
    
    for (char c : key) {
        hash = (hash * base + c) % tableSize;
    }
    return hash;
}
```

#### 4. **Universal Hashing**
```cpp
class UniversalHash {
private:
    int a, b, p, m;  // p is prime > max key, m is table size
    
public:
    UniversalHash(int tableSize) : m(tableSize) {
        p = 1000000007;  // Large prime
        a = rand() % (p - 1) + 1;  // 1 <= a < p
        b = rand() % p;            // 0 <= b < p
    }
    
    int hash(int key) {
        return ((a * key + b) % p) % m;
    }
};
```

## Time Complexity

### **Average Case Analysis**

| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| **Insert** | O(1) | O(n) |
| **Search** | O(1) | O(n) |
| **Delete** | O(1) | O(n) |

### **Why Average Case is O(1)?**

Assuming a good hash function with uniform distribution:

1. **Hash Function**: O(k) where k is key length
2. **Array Access**: O(1) - direct indexing
3. **Collision Resolution**: O(α) where α is load factor

**Load Factor**: α = n/m (number of elements / table size)

### **Load Factor Impact**

```cpp
class HashTable {
private:
    vector<list<pair<string, int>>> table;  // Chaining
    int size;
    int count;
    double loadFactor;
    
public:
    void insert(string key, int value) {
        if (loadFactor > 0.75) {  // Threshold
            resize();
        }
        
        int index = hashFunction(key);
        table[index].push_back({key, value});
        count++;
        loadFactor = (double)count / size;
    }
    
    void resize() {
        vector<list<pair<string, int>>> oldTable = table;
        size *= 2;
        table = vector<list<pair<string, int>>>(size);
        count = 0;
        
        // Rehash all elements
        for (auto& bucket : oldTable) {
            for (auto& pair : bucket) {
                insert(pair.first, pair.second);
            }
        }
    }
};
```

### **Amortized Analysis**

**Amortized analysis** is a method of analyzing the time complexity of operations where some operations might be expensive, but they happen infrequently enough that the **average cost per operation** is still good.

#### **Key Concept:**
- **Individual operations** might have different costs
- **Some operations** are expensive (like resizing)
- **Most operations** are cheap (like normal insert)
- **Amortized cost** = average cost per operation over a sequence of operations

#### **Hash Table Resizing Example:**

Let's trace through inserting elements 1, 2, 3, 4, 5, 6, 7, 8 in a hash table that starts with size 4:

```cpp
class HashTable {
private:
    vector<list<pair<string, int>>> table;
    int size;
    int count;
    
public:
    void insert(string key, int value) {
        if (count >= size * 0.75) {  // Load factor threshold
            resize();  // This is expensive!
        }
        
        // Normal insert - this is cheap
        int index = hashFunction(key);
        table[index].push_back({key, value});
        count++;
    }
    
    void resize() {
        // This is O(n) - expensive!
        vector<list<pair<string, int>>> oldTable = table;
        size *= 2;
        table = vector<list<pair<string, int>>>(size);
        count = 0;
        
        // Rehash all elements
        for (auto& bucket : oldTable) {
            for (auto& pair : bucket) {
                insert(pair.first, pair.second);
            }
        }
    }
};
```

#### **Operation Sequence Analysis:**
```
Insert 1: O(1) - normal insert
Insert 2: O(1) - normal insert  
Insert 3: O(1) - normal insert
Insert 4: O(1) - normal insert
Insert 5: O(4) - triggers resize! (rehashes elements 1,2,3,4)
Insert 6: O(1) - normal insert
Insert 7: O(1) - normal insert
Insert 8: O(1) - normal insert
Insert 9: O(8) - triggers resize! (rehashes elements 1,2,3,4,5,6,7,8)
```

#### **Cost Calculation:**
- **Total cost**: 1 + 1 + 1 + 1 + 4 + 1 + 1 + 1 + 8 = 19 operations
- **Number of operations**: 9 inserts
- **Amortized cost per operation**: 19 ÷ 9 ≈ 2.1 ≈ O(1)

#### **Why Amortized Analysis Works:**

**The Pattern:**
1. **Most operations** (like 7 out of 9) cost O(1)
2. **Expensive operations** (resize) happen rarely
3. **When resize happens**, it's because we've done many cheap operations
4. **The expensive cost is "spread out"** over all the previous cheap operations

**Mathematical Insight:**
```
Resize happens when: count = size
After resize: size = 2 * old_size

So we had: old_size cheap operations before this expensive resize
The expensive resize cost: O(old_size)
Amortized cost: O(old_size) / old_size = O(1)
```

#### **Amortized vs Average Case:**

| Aspect | Average Case | Amortized |
|--------|--------------|-----------|
| **Basis** | Random inputs | Worst-case sequence |
| **Distribution** | Assumes uniform | Any sequence |
| **Guarantee** | Probabilistic | Deterministic |
| **Analysis** | Statistical | Mathematical |

#### **Real-World Analogy:**
Think of it like **buying groceries**:
- **Most days**: You buy a few items for $10-20 (cheap operations)
- **Occasionally**: You do a big shopping trip for $200 (expensive operation)
- **Amortized cost**: Over a month, your average daily grocery cost might be $15

The expensive shopping trip doesn't make your daily average terrible because it happens infrequently.

#### **Other Examples of Amortized Analysis:**

**1. Dynamic Array (Vector):**
```cpp
vector<int> vec;
// Most push_back operations: O(1)
// Occasionally: O(n) when resizing
// Amortized: O(1) per push_back
```

**2. Binary Counter:**
```cpp
// Incrementing a binary counter
// Most increments: O(1) - just flip a bit
// Occasionally: O(log n) - when all bits flip
// Amortized: O(1) per increment
```

**3. Disjoint Set Union (DSU):**
```cpp
// Most operations: O(α(n)) where α is inverse Ackermann
// Amortized: O(α(n)) per operation
// α(n) is practically constant for reasonable n
```

#### **Key Takeaways:**
- **Amortized ≠ Average**: Amortized is worst-case over sequences, average is probabilistic
- **Expensive operations are rare**: They happen infrequently enough to not dominate
- **Cost is "spread out"**: The expensive cost is distributed over many cheap operations
- **Guaranteed performance**: Unlike average case, amortized gives worst-case guarantees
- **Practical importance**: Many real-world data structures rely on amortized analysis

With dynamic resizing:
- **Single operation**: O(1) average
- **Resize operation**: O(n) but happens infrequently
- **Amortized cost**: O(1) per operation

## Collisions

### **What are Collisions?**

A **collision** occurs when two different keys produce the same hash value (same array index).

```cpp
// Example collision
string key1 = "hello";
string key2 = "world";

// If both hash to index 5, we have a collision
int index1 = hashFunction(key1);  // Returns 5
int index2 = hashFunction(key2);  // Also returns 5
```

### **Why Collisions Happen**

1. **Pigeonhole Principle**: More keys than available slots
2. **Hash Function Limitations**: No perfect hash function exists for arbitrary keys
3. **Load Factor**: Higher load factor increases collision probability

### **Collision Resolution Techniques**

#### 1. **Chaining (Separate Chaining)**

**Concept**: Store multiple elements in the same slot using a linked list.

```cpp
class ChainingHashTable {
private:
    vector<list<pair<string, int>>> table;
    int size;
    
public:
    ChainingHashTable(int s) : size(s), table(s) {}
    
    void insert(string key, int value) {
        int index = hashFunction(key);
        
        // Check if key already exists
        for (auto it = table[index].begin(); it != table[index].end(); ++it) {
            if (it->first == key) {
                it->second = value;  // Update existing
                return;
            }
        }
        
        // Add new key-value pair
        table[index].push_back({key, value});
    }
    
    int search(string key) {
        int index = hashFunction(key);
        
        for (auto& pair : table[index]) {
            if (pair.first == key) {
                return pair.second;
            }
        }
        return -1;  // Not found
    }
    
    void remove(string key) {
        int index = hashFunction(key);
        
        for (auto it = table[index].begin(); it != table[index].end(); ++it) {
            if (it->first == key) {
                table[index].erase(it);
                return;
            }
        }
    }
};
```

**Time Complexity**:
- **Average**: O(1 + α) where α is load factor
- **Worst**: O(n) if all keys hash to same slot

#### 2. **Open Addressing**

**Concept**: Find another slot in the table when collision occurs.

##### **Linear Probing**
```cpp
class LinearProbingHashTable {
private:
    vector<pair<string, int>> table;
    vector<bool> occupied;
    int size;
    
public:
    LinearProbingHashTable(int s) : size(s), table(s), occupied(s, false) {}
    
    void insert(string key, int value) {
        int index = hashFunction(key);
        
        // Find next available slot
        while (occupied[index] && table[index].first != key) {
            index = (index + 1) % size;
        }
        
        table[index] = {key, value};
        occupied[index] = true;
    }
    
    int search(string key) {
        int index = hashFunction(key);
        int originalIndex = index;
        
        while (occupied[index]) {
            if (table[index].first == key) {
                return table[index].second;
            }
            index = (index + 1) % size;
            
            // Prevent infinite loop
            if (index == originalIndex) break;
        }
        
        return -1;  // Not found
    }
};
```

##### **Quadratic Probing**
```cpp
int quadraticProbe(int originalIndex, int i, int size) {
    return (originalIndex + i * i) % size;
}

void insert(string key, int value) {
    int index = hashFunction(key);
    int originalIndex = index;
    int i = 0;
    
    while (occupied[index] && table[index].first != key) {
        i++;
        index = quadraticProbe(originalIndex, i, size);
    }
    
    table[index] = {key, value};
    occupied[index] = true;
}
```

##### **Double Hashing**
```cpp
class DoubleHashingHashTable {
private:
    vector<pair<string, int>> table;
    vector<bool> occupied;
    int size;
    
    int hashFunction1(string key) {
        // Primary hash function
        int hash = 0;
        for (char c : key) {
            hash = (hash * 31 + c) % size;
        }
        return hash;
    }
    
    int hashFunction2(string key) {
        // Secondary hash function
        int hash = 0;
        for (char c : key) {
            hash = (hash * 37 + c) % size;
        }
        return (hash % (size - 1)) + 1;  // Must be coprime with size
    }
    
public:
    void insert(string key, int value) {
        int index = hashFunction1(key);
        int step = hashFunction2(key);
        int originalIndex = index;
        
        while (occupied[index] && table[index].first != key) {
            index = (index + step) % size;
            
            // Prevent infinite loop
            if (index == originalIndex) {
                // Table is full, need to resize
                resize();
                insert(key, value);
                return;
            }
        }
        
        table[index] = {key, value};
        occupied[index] = true;
    }
};
```

### **Collision Resolution Comparison**

| Method | Average Case | Worst Case | Pros | Cons |
|--------|--------------|------------|------|------|
| **Chaining** | O(1 + α) | O(n) | Simple, no clustering | Extra memory for pointers |
| **Linear Probing** | O(1/(1-α)) | O(n) | Cache friendly | Primary clustering |
| **Quadratic Probing** | O(1/(1-α)) | O(n) | Reduces clustering | Secondary clustering |
| **Double Hashing** | O(1/(1-α)) | O(n) | Best distribution | More complex |

## Real-World Applications

### 1. **Database Indexing**
```cpp
// B-tree with hash indexing
class DatabaseIndex {
private:
    unordered_map<string, vector<int>> index;  // Key -> Row IDs
    
public:
    void insert(string key, int rowId) {
        index[key].push_back(rowId);
    }
    
    vector<int> search(string key) {
        return index[key];
    }
};
```

### 2. **Caching (LRU Cache)**
```cpp
class LRUCache {
private:
    unordered_map<int, list<pair<int, int>>::iterator> cache;
    list<pair<int, int>> items;
    int capacity;
    
public:
    LRUCache(int cap) : capacity(cap) {}
    
    int get(int key) {
        auto it = cache.find(key);
        if (it != cache.end()) {
            // Move to front (most recently used)
            items.splice(items.begin(), items, it->second);
            return it->second->second;
        }
        return -1;
    }
    
    void put(int key, int value) {
        auto it = cache.find(key);
        
        if (it != cache.end()) {
            // Update existing
            it->second->second = value;
            items.splice(items.begin(), items, it->second);
        } else {
            // Add new
            if (cache.size() >= capacity) {
                // Remove least recently used
                int lruKey = items.back().first;
                items.pop_back();
                cache.erase(lruKey);
            }
            
            items.push_front({key, value});
            cache[key] = items.begin();
        }
    }
};
```

### 3. **Symbol Tables in Compilers**
```cpp
class SymbolTable {
private:
    unordered_map<string, SymbolInfo> symbols;
    
public:
    void addSymbol(string name, string type, int scope) {
        symbols[name] = {type, scope};
    }
    
    SymbolInfo lookup(string name) {
        return symbols[name];
    }
};
```

### 4. **Distributed Systems (Consistent Hashing)**
```cpp
class ConsistentHash {
private:
    map<int, string> ring;  // Hash value -> Server name
    vector<int> serverHashes;
    
public:
    void addServer(string serverName) {
        int hash = hashFunction(serverName);
        ring[hash] = serverName;
        serverHashes.push_back(hash);
        sort(serverHashes.begin(), serverHashes.end());
    }
    
    string getServer(string key) {
        int keyHash = hashFunction(key);
        
        // Find first server with hash >= keyHash
        auto it = lower_bound(serverHashes.begin(), serverHashes.end(), keyHash);
        
        if (it == serverHashes.end()) {
            // Wrap around to first server
            return ring[serverHashes[0]];
        }
        
        return ring[*it];
    }
};
```

## Performance Optimization

### **Load Factor Management**
```cpp
class OptimizedHashTable {
private:
    static constexpr double MAX_LOAD_FACTOR = 0.75;
    static constexpr double MIN_LOAD_FACTOR = 0.25;
    
    void checkAndResize() {
        double loadFactor = (double)count / size;
        
        if (loadFactor > MAX_LOAD_FACTOR) {
            resize(size * 2);
        } else if (loadFactor < MIN_LOAD_FACTOR && size > 4) {
            resize(size / 2);
        }
    }
};
```

### **Hash Function Optimization**
```cpp
// Fast string hashing for short strings
int fastHash(string key) {
    if (key.length() <= 4) {
        // Use all characters for short strings
        int hash = 0;
        for (char c : key) {
            hash = (hash << 8) | c;  // Bit shifting
        }
        return hash;
    } else {
        // Use polynomial rolling for longer strings
        int hash = 0;
        for (char c : key) {
            hash = hash * 31 + c;
        }
        return hash;
    }
}
```

## Common Pitfalls

### 1. **Poor Hash Function**
```cpp
// BAD: Only uses first character
int badHash(string key) {
    return key[0] % size;  // Many collisions!
}

// GOOD: Uses all characters
int goodHash(string key) {
    int hash = 0;
    for (char c : key) {
        hash = (hash * 31 + c) % size;
    }
    return hash;
}
```

### 2. **Not Handling Load Factor**
```cpp
// BAD: No resizing
class BadHashTable {
    // Fixed size, no dynamic resizing
    // Performance degrades as table fills up
};

// GOOD: Dynamic resizing
class GoodHashTable {
    void checkLoadFactor() {
        if (loadFactor > 0.75) {
            resize();
        }
    }
};
```

### 3. **Memory Leaks in Chaining**
```cpp
// BAD: Not cleaning up
~HashTable() {
    // Forgot to delete nodes in chains
}

// GOOD: Proper cleanup
~HashTable() {
    for (auto& bucket : table) {
        // Clear the list (automatic cleanup)
        bucket.clear();
    }
}
```

## Summary

**Hashing** provides:
- **Average O(1)** time complexity for insert, search, delete
- **Fast data retrieval** through direct array access
- **Flexible key types** with proper hash functions

**Key Considerations**:
- **Hash function quality** affects performance
- **Collision resolution** is essential
- **Load factor management** prevents degradation
- **Memory vs speed** trade-offs

**Best Practices**:
- Use good hash functions with uniform distribution
- Monitor and manage load factor
- Choose appropriate collision resolution method
- Consider memory overhead vs performance benefits


### Practice Problem:
- [Two Sum](https://leetcode.com/problems/two-sum/)
- [Four Sum](https://leetcode.com/problems/4sum/)
- [Four Sum ii](https://leetcode.com/problems/4sum-ii/)
- [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/description/)
- [Largest Subarray With 0 Sum](https://www.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1)
- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/submissions/1772075664/)
- [Continous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/description/)
- [Count Subarray With Given Xor](https://www.geeksforgeeks.org/problems/count-subarray-with-given-xor/1#)
- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/submissions/1773255078/)
- [Longest Subarry With Sum K](https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1)
- [Product Of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
- [Construct Product Matrix](https://leetcode.com/problems/construct-product-matrix/)