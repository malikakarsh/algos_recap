# Linked Lists

## What is a Linked List?

A **linked list** is a linear data structure where elements (nodes) are stored in sequence, but unlike arrays, the elements are not stored in contiguous memory locations. Instead, each node contains:
- **Data**: The actual value
- **Pointer/Reference**: Address of the next node

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};
```

## Types of Linked Lists

### 1. Singly Linked List
- Each node points to the next node only
- Can only traverse in one direction (forward)
- Most common type

```cpp
struct SinglyNode {
    int data;
    SinglyNode* next;
};
```

### 2. Doubly Linked List
- Each node has pointers to both next and previous nodes
- Can traverse in both directions
- Takes more memory but offers more flexibility

```cpp
struct DoublyNode {
    int data;
    DoublyNode* next;
    DoublyNode* prev;
};
```

### 3. Circular Linked List
- Last node points back to the first node
- Can be singly or doubly circular
- Useful for round-robin algorithms

```cpp
// Circular singly linked list
ListNode* head = new ListNode(1);
head->next = new ListNode(2);
head->next->next = new ListNode(3);
head->next->next->next = head;  // Points back to head
```

## Linked List vs Array

| Feature | Array | Linked List |
|---------|-------|-------------|
| **Memory** | Contiguous | Non-contiguous |
| **Access Time** | O(1) - Random access | O(n) - Sequential access |
| **Insertion** | O(n) - May need shifting | O(1) - If position known |
| **Deletion** | O(n) - May need shifting | O(1) - If position known |
| **Memory Overhead** | None | Extra pointer(s) per node |
| **Cache Performance** | Better (locality) | Worse (scattered) |

## Real-World Applications

### 1. **Music Playlist**
```cpp
struct Song {
    string title;
    string artist;
    Song* next;
};

class Playlist {
    Song* head;
public:
    void addSong(string title, string artist) {
        Song* newSong = new Song{title, artist, head};
        head = newSong;
    }
    
    void playNext() {
        if (head) {
            cout << "Playing: " << head->title << " by " << head->artist << endl;
            head = head->next;
        }
    }
};
```

### 2. **Browser History**
- Each page visit is a node
- Forward/backward navigation uses doubly linked list
- Recent pages at the front, older pages further back

### 3. **Undo/Redo Functionality**
- Each action is stored as a node
- Undo moves backward, redo moves forward
- Perfect use case for doubly linked list

### 4. **File System Directory Structure**
- Folders and files can be represented as nodes
- Parent-child relationships maintained through pointers

### 5. **Social Media Feeds**
- Posts appear in chronological order
- New posts added to the front
- Scrolling loads more content (lazy loading)

### 6. **CPU Process Scheduling**
- Processes in ready queue
- Round-robin scheduling uses circular linked list
- Dynamic addition/removal of processes

## Common Operations

### Insertion

#### Insert at Beginning
```cpp
void insertAtHead(ListNode*& head, int val) {
    ListNode* newNode = new ListNode(val);
    newNode->next = head;
    head = newNode;
}
```

#### Insert at End
```cpp
void insertAtTail(ListNode*& head, int val) {
    ListNode* newNode = new ListNode(val);
    
    if (!head) {
        head = newNode;
        return;
    }
    
    ListNode* current = head;
    while (current->next) {
        current = current->next;
    }
    current->next = newNode;
}
```

#### Insert at Position
```cpp
void insertAtPosition(ListNode*& head, int val, int pos) {
    if (pos == 0) {
        insertAtHead(head, val);
        return;
    }
    
    ListNode* newNode = new ListNode(val);
    ListNode* current = head;
    
    for (int i = 0; i < pos - 1 && current; i++) {
        current = current->next;
    }
    
    if (current) {
        newNode->next = current->next;
        current->next = newNode;
    }
}
```

### Deletion

#### Delete by Value
```cpp
void deleteByValue(ListNode*& head, int val) {
    if (!head) return;
    
    if (head->val == val) {
        ListNode* temp = head;
        head = head->next;
        delete temp;
        return;
    }
    
    ListNode* current = head;
    while (current->next && current->next->val != val) {
        current = current->next;
    }
    
    if (current->next) {
        ListNode* temp = current->next;
        current->next = current->next->next;
        delete temp;
    }
}
```

### Traversal
```cpp
void printList(ListNode* head) {
    ListNode* current = head;
    while (current) {
        cout << current->val << " -> ";
        current = current->next;
    }
    cout << "null" << endl;
}
```

## Are Stacks and Queues Linked Lists?

### **Short Answer**: Stacks and Queues are **abstract data types** that can be implemented using various underlying data structures, including linked lists.

### **Detailed Explanation**:

#### **Stack Implementation Options**:

**1. Array-based Stack**:
```cpp
class ArrayStack {
    vector<int> data;
public:
    void push(int val) { data.push_back(val); }
    int pop() { 
        int val = data.back();
        data.pop_back();
        return val;
    }
    int top() { return data.back(); }
};
```

**2. Linked List-based Stack**:
```cpp
class LinkedListStack {
    ListNode* head;
public:
    void push(int val) {
        ListNode* newNode = new ListNode(val);
        newNode->next = head;
        head = newNode;
    }
    
    int pop() {
        if (!head) throw runtime_error("Stack empty");
        int val = head->val;
        ListNode* temp = head;
        head = head->next;
        delete temp;
        return val;
    }
    
    int top() {
        if (!head) throw runtime_error("Stack empty");
        return head->val;
    }
};
```

#### **Queue Implementation Options**:

**1. Array-based Queue** (with circular buffer):
```cpp
class ArrayQueue {
    vector<int> data;
    int front, rear, size;
public:
    void enqueue(int val) {
        if (size == data.size()) throw runtime_error("Queue full");
        data[rear] = val;
        rear = (rear + 1) % data.size();
        size++;
    }
    
    int dequeue() {
        if (size == 0) throw runtime_error("Queue empty");
        int val = data[front];
        front = (front + 1) % data.size();
        size--;
        return val;
    }
};
```

**2. Linked List-based Queue**:
```cpp
class LinkedListQueue {
    ListNode* front;
    ListNode* rear;
public:
    void enqueue(int val) {
        ListNode* newNode = new ListNode(val);
        if (!rear) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }
    }
    
    int dequeue() {
        if (!front) throw runtime_error("Queue empty");
        int val = front->val;
        ListNode* temp = front;
        front = front->next;
        if (!front) rear = nullptr;
        delete temp;
        return val;
    }
};
```

### **When to Use Each Implementation**:

| Implementation | Pros | Cons | Best For |
|----------------|------|------|----------|
| **Array Stack** | Cache-friendly, simple | Fixed size, expensive resizing | Known size limits |
| **Linked List Stack** | Dynamic size, O(1) operations | Extra memory overhead | Unknown size, frequent operations |
| **Array Queue** | Cache-friendly | Fixed size, complex circular logic | Known size limits |
| **Linked List Queue** | Dynamic size, simple logic | Extra memory overhead | Unknown size, frequent operations |

## Advanced Linked List Concepts

### **Floyd's Cycle Detection (Tortoise and Hare)**
```cpp
bool hasCycle(ListNode* head) {
    if (!head || !head->next) return false;
    
    ListNode* slow = head;
    ListNode* fast = head->next;
    
    while (fast && fast->next) {
        if (slow == fast) return true;
        slow = slow->next;
        fast = fast->next->next;
    }
    
    return false;
}
```

### **Merge Two Sorted Lists**
```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    ListNode dummy(0);
    ListNode* current = &dummy;
    
    while (list1 && list2) {
        if (list1->val <= list2->val) {
            current->next = list1;
            list1 = list1->next;
        } else {
            current->next = list2;
            list2 = list2->next;
        }
        current = current->next;
    }
    
    current->next = list1 ? list1 : list2;
    return dummy.next;
}
```

### **Reverse Linked List**
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* current = head;
    
    while (current) {
        ListNode* next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    
    return prev;
}
```

## Memory Management Best Practices

### **Always Delete Nodes**
```cpp
void deleteList(ListNode*& head) {
    while (head) {
        ListNode* temp = head;
        head = head->next;
        delete temp;
    }
}
```

### **Use Smart Pointers (Modern C++)**
```cpp
struct SmartListNode {
    int val;
    unique_ptr<SmartListNode> next;
    SmartListNode(int x) : val(x), next(nullptr) {}
};
```

## Common Interview Questions

1. **Reverse a linked list**
2. **Detect cycle in linked list**
3. **Find middle element**
4. **Merge two sorted lists**
5. **Remove duplicates**
6. **Intersection of two lists**
7. **Palindrome linked list**
8. **Add two numbers represented as linked lists**

## Summary

Linked lists are fundamental data structures that offer:
- **Dynamic size**: No need to know size in advance
- **Efficient insertion/deletion**: O(1) at known positions
- **Memory efficiency**: Only allocate what you need
- **Flexibility**: Easy to modify structure

**Key Takeaway**: Stacks and Queues are **concepts** that can be implemented using arrays, linked lists, or other data structures. Linked lists are particularly useful when you need dynamic sizing and frequent insertions/deletions.
