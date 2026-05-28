# Trie

## Implementation

```cpp
struct Node{
    Node* links[26];
    bool flag = false;
};
class Trie {
private:
    Node* root;
public:
    Trie() {
        root = new Node();
    }
    
    void insert(string word) {
        Node* node = root;
        for (auto ch:word){
            if (node->links[ch-'a'] == NULL){
                node->links[ch-'a'] = new Node();
            }
            node = node->links[ch-'a'];
        }
        node->flag = true;
    }
    
    bool search(string word) {
        Node* node = root;
        for (auto ch:word){
            if (node->links[ch-'a'] == NULL) return false;
            node = node->links[ch-'a'];
        }
        return node->flag == true;
    }
    
    bool startsWith(string prefix) {
        Node* node = root;
        for (auto ch:prefix){
            if (node->links[ch-'a'] == NULL) return false;
            node = node->links[ch-'a'];
        }
        return true;
    }
};
```

## Practice Problems

- [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)