## Binary Trees: Key Types and Examples

A binary tree is a hierarchical structure where each node has at most two children: left and right.

---

### Full Binary Tree
Definition: Every node has either 0 or 2 children (no node with only one child).

Example (Full, not necessarily complete):
```
      1
     / \
    2   3
       / \
      4   5
```

---

## Constructing a Binary Tree from Traversal Arrays

Key rules for reconstruction:
- Preorder + Inorder → unique tree
- Postorder + Inorder → unique tree
- Preorder + Postorder → generally not unique; unique if the tree is full (every node has 0 or 2 children)

Use a hash map from value → inorder index for O(1) splits. All code below assumes node values are unique. If values can repeat, you need disambiguation (e.g., annotate with indices).

### Build from Preorder + Inorder
- Preorder: Root, Left, Right → the first element is the root
- Inorder: Left, Root, Right → split by the root’s position:
  - Left subtree = inorder[l .. k-1]
  - Right subtree = inorder[k+1 .. r]

```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

TreeNode* buildPreIn(const vector<int>& preorder, int& preIdx,
                     int inL, int inR,
                     const unordered_map<int,int>& inPos) {
    if (inL > inR) return nullptr;
    int rootVal = preorder[preIdx++];
    int k = inPos.at(rootVal);           // root position in inorder
    TreeNode* root = new TreeNode(rootVal);
    root->left = buildPreIn(preorder, preIdx, inL, k - 1, inPos);   // left half
    root->right = buildPreIn(preorder, preIdx, k + 1, inR, inPos);  // right half
    return root;
}

TreeNode* buildTreeFromPreIn(const vector<int>& preorder, const vector<int>& inorder) {
    unordered_map<int,int> inPos;
    for (int i = 0; i < (int)inorder.size(); ++i) inPos[inorder[i]] = i;
    int preIdx = 0;
    return buildPreIn(preorder, preIdx, 0, (int)inorder.size() - 1, inPos);
}
```
### Practice Problems:
- [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
---

## Diameter of a Binary Tree

Definition: The diameter is the length (number of edges) of the longest path between any two nodes in the tree. The path may or may not pass through the root.

Idea: Do a single DFS that returns height, and at each node consider `leftHeight + rightHeight` as a candidate diameter.

```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

int dfsHeight(TreeNode* node, int& diam) {
    if (!node) return 0;
    int hl = dfsHeight(node->left, diam);
    int hr = dfsHeight(node->right, diam);
    diam = max(diam, hl + hr);       // longest path through this node (edges)
    return 1 + max(hl, hr);          // height in nodes; edges height not required
}

int diameterOfBinaryTree(TreeNode* root) {
    int diam = 0;
    dfsHeight(root, diam);
    return diam;
}
```

### Practice Problems
- [Diameter Of A Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/description/)
- [Binary Tree Tilt](https://leetcode.com/problems/binary-tree-tilt/submissions/1784675500/)

### Build from Postorder + Inorder
- Postorder: Left, Right, Root → the last element is the root
- Inorder: Left, Root, Right → split by the root’s position:
  - Left subtree = inorder[l .. k-1]
  - Right subtree = inorder[k+1 .. r]
- Important: Build right subtree first, then left (because we consume postorder from the end).

```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

TreeNode* buildPostIn(const vector<int>& postorder, int& postIdx,
                      int inL, int inR,
                      const unordered_map<int,int>& inPos) {
    if (inL > inR) return nullptr;
    int rootVal = postorder[postIdx--];
    int k = inPos.at(rootVal);
    TreeNode* root = new TreeNode(rootVal);
    root->right = buildPostIn(postorder, postIdx, k + 1, inR, inPos); // right half first
    root->left  = buildPostIn(postorder, postIdx, inL, k - 1, inPos); // left half
    return root;
}

TreeNode* buildTreeFromPostIn(const vector<int>& postorder, const vector<int>& inorder) {
    unordered_map<int,int> inPos;
    for (int i = 0; i < (int)inorder.size(); ++i) inPos[inorder[i]] = i;
    int postIdx = (int)postorder.size() - 1;
    return buildPostIn(postorder, postIdx, 0, (int)inorder.size() - 1, inPos);
}
```

### Build from Preorder + Postorder (Full Binary Tree)
- Preorder: Root, Left, Right (consume from start)
- Postorder: Left, Right, Root (use to determine left subtree boundary)
- Not unique in general. For full binary trees, we can reconstruct by finding where the left subtree ends in postorder.

Algorithm:
1) The first preorder element is root
2) If root is a leaf (or segment size 1), return
3) The next preorder element is the left child’s root. Find it in postorder to get the left subtree size
4) Recurse for left and right using that size

```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

TreeNode* buildPrePost(const vector<int>& preorder, int& preIdx,
                       const vector<int>& postorder, int postL, int postR,
                       const unordered_map<int,int>& postPos) {
    TreeNode* root = new TreeNode(preorder[preIdx++]);
    if (postL == postR) return root; // leaf
    // Next preorder value is the left child’s root
    int leftRootVal = preorder[preIdx];
    int k = postPos.at(leftRootVal); // left subtree ends at k in postorder
    int leftSize = k - postL + 1;
    // Build left using postL..k, then right using k+1..postR-1 (exclude root at postR)
    root->left  = buildPrePost(preorder, preIdx, postorder, postL, k, postPos);
    root->right = buildPrePost(preorder, preIdx, postorder, k + 1, postR - 1, postPos);
    return root;
}

TreeNode* buildTreeFromPrePost(const vector<int>& preorder, const vector<int>& postorder) {
    if (preorder.empty()) return nullptr;
    unordered_map<int,int> postPos;
    for (int i = 0; i < (int)postorder.size(); ++i) postPos[postorder[i]] = i;
    int preIdx = 0;
    return buildPrePost(preorder, preIdx, postorder, 0, (int)postorder.size() - 1, postPos);
}
```

### Complete Binary Tree (CBT)
Definition: All levels are completely filled except possibly the last, and the last level’s nodes are as far left as possible.

Example (array-like layout):
```
      1
     / \
    2   3
   / \  /
  4  5 6
```

---

### Perfect Binary Tree
Definition: All internal nodes have exactly two children and all leaves are at the same depth.

Example:
```
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

---

### Balanced Binary Tree
Definition: Height of the two subtrees of every node differs by at most 1 (AVL definition). More generally, “balanced” means height grows O(log N).

Example (height-balanced):
```
      10
     /  \
    5    15
   / \     \
  3   7     18
```

---

### Degenerate (Skewed) Binary Tree
Definition: Each parent has only one child; effectively behaves like a linked list.

Examples:
Left-skewed:
```
    1
   /
  2
 /
3
```

Right-skewed:
```
1
 \
  2
   \
    3
```

---

## Traversal Techniques

How to remember the root position:
- Inorder: root in the middle (Left, Root, Right)
- Preorder: root at the left (Root, Left, Right)
- Postorder: root at the extreme right (Left, Right, Root)

Example tree:
```
      1
     / \
    2   3
   / \
  4   5
```

Orders on the example:
- Inorder (L, R, R): 4, 2, 5, 1, 3
- Preorder (R, L, R): 1, 2, 4, 5, 3
- Postorder (L, R, R): 4, 5, 2, 3, 1


### Iterative Preorder (using stack)
```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

vector<int> preorderTraversal(TreeNode* root) {
    vector<int> out;
    if (!root) return out;
    stack<TreeNode*> st;
    st.push(root);
    while (!st.empty()) {
        TreeNode* node = st.top(); st.pop();
        out.push_back(node->val);          // visit root
        if (node->right) st.push(node->right); // push right first so left is processed first
        if (node->left) st.push(node->left);
    }
    return out;
}
```

### Iterative Inorder (using stack)
```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> out;
    stack<TreeNode*> st;
    TreeNode* curr = root;
    while (curr || !st.empty()) {
        while (curr) {              // go left as far as possible
            st.push(curr);
            curr = curr->left;
        }
        curr = st.top(); st.pop();  // visit node
        out.push_back(curr->val);
        curr = curr->right;         // then go right
    }
    return out;
}
```

### Iterative Postorder (two stacks)
```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

vector<int> postorderTraversalTwoStacks(TreeNode* root) {
    vector<int> out;
    if (!root) return out;
    stack<TreeNode*> s1, s2;
    s1.push(root);
    while (!s1.empty()) {
        TreeNode* node = s1.top(); s1.pop();
        s2.push(node);
        if (node->left) s1.push(node->left);
        if (node->right) s1.push(node->right);
    }
    while (!s2.empty()) {
        out.push_back(s2.top()->val);
        s2.pop();
    }
    return out;
}
```

### Iterative Postorder (using stack)
```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

vector<int> postorderTraversal(TreeNode* root) {
    vector<int> out;
    stack<TreeNode*> st;
    TreeNode* curr = root;
    TreeNode* lastVisited = nullptr;
    while (curr || !st.empty()) {
        if (curr) {
            st.push(curr);
            curr = curr->left;
        } else {
            TreeNode* node = st.top();
            if (node->right && lastVisited != node->right) {
                curr = node->right; // go right if not processed
            } else {
                out.push_back(node->val); // visit
                st.pop();
                lastVisited = node;
            }
        }
    }
    return out;
}
```

### All three traversals in one pass (using a single stack)
```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

// Returns {preorder, inorder, postorder}
vector<vector<int>> allTraversals(TreeNode* root) {
    vector<int> pre, in, post;
    if (!root) return {pre, in, post};
    stack<pair<TreeNode*, int>> st; // state: 1=pre, 2=in, 3=post
    st.push({root, 1});
    while (!st.empty()) {
        auto [node, state] = st.top();
        st.pop();
        if (state == 1) {
            pre.push_back(node->val);        // preorder when first seen
            st.push({node, 2});              // next time process inorder
            if (node->left) st.push({node->left, 1});
        } else if (state == 2) {
            in.push_back(node->val);         // inorder
            st.push({node, 3});              // next time process postorder
            if (node->right) st.push({node->right, 1});
        } else {
            post.push_back(node->val);       // postorder
        }
    }
    return {pre, in, post};
}
```

---

## Generating Traversals from a Sorted Array (Balanced BST via mid-splits)

Given a sorted array, you can conceptually build a height-balanced BST by picking the middle element as root, the left half as the left subtree, and the right half as the right subtree. Then, producing preorder/inorder/postorder is just about the visit order. Below, each snippet shows which half maps to left vs right.

### Inorder from sorted array
Inorder of the balanced BST is simply the original sorted array (Left, Root, Right order keeps it sorted). For completeness, here is the recursive structure:
```cpp
void inorderFromSorted(const vector<int>& a, int l, int r, vector<int>& out) {
    if (l > r) return;
    int m = l + (r - l) / 2;        // root = a[m]
    inorderFromSorted(a, l, m - 1, out);  // left half
    out.push_back(a[m]);                   // root
    inorderFromSorted(a, m + 1, r, out);  // right half
}
```

### Preorder from sorted array (balanced BST)
Preorder is Root, Left, Right. Choose mid as root, then recurse on halves:
```cpp
void preorderFromSorted(const vector<int>& a, int l, int r, vector<int>& out) {
    if (l > r) return;
    int m = l + (r - l) / 2;        // root = a[m]
    out.push_back(a[m]);                   // root
    preorderFromSorted(a, l, m - 1, out);  // left half
    preorderFromSorted(a, m + 1, r, out);  // right half
}
```

### Postorder from sorted array (balanced BST)
Postorder is Left, Right, Root. Same halves, different visit order:
```cpp
void postorderFromSorted(const vector<int>& a, int l, int r, vector<int>& out) {
    if (l > r) return;
    int m = l + (r - l) / 2;        // root = a[m]
    postorderFromSorted(a, l, m - 1, out); // left half
    postorderFromSorted(a, m + 1, r, out); // right half
    out.push_back(a[m]);                    // root
}
```

Usage example:
```cpp
vector<int> a = {1,2,3,4,5,6,7};
vector<int> pre, in, post;
preorderFromSorted(a, 0, (int)a.size()-1, pre);
inorderFromSorted(a, 0, (int)a.size()-1, in);
postorderFromSorted(a, 0, (int)a.size()-1, post);
// in = {1,2,3,4,5,6,7}
// pre = {4,2,1,3,6,5,7}
// post = {1,3,2,5,7,6,4}
```


### Practice Prbolems:
- [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/submissions/1783703772/)
- [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
- [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/submissions/1783705392/)
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/submissions/1784234494/)
- [Maximum Depth Of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/submissions/1784614638/)

---

## Balanced Binary Tree: Definition and Check

Definition: A binary tree is height-balanced if for every node, the absolute difference between the heights of its left and right subtrees is at most 1.

Efficient check (O(N)): Compute height bottom-up and propagate an invalid signal (e.g., -1) when a subtree is unbalanced. If any subtree is unbalanced, short-circuit upwards.

```cpp
struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

int heightOrFail(TreeNode* node) {
    if (!node) return 0;
    int hl = heightOrFail(node->left);
    if (hl == -1) return -1;
    int hr = heightOrFail(node->right);
    if (hr == -1) return -1;
    if (abs(hl - hr) > 1) return -1;
    return 1 + max(hl, hr);
}

bool isBalanced(TreeNode* root) {
    return heightOrFail(root) != -1;
}
```

### Practice Problem
- [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
- [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
- [Same Tree](https://leetcode.com/problems/same-tree/)
- [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
- [Boundary Traversal Of Binary Tree](https://www.geeksforgeeks.org/problems/boundary-traversal-of-binary-tree/)
- [Top View Of Binary Tree](https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/)
- [Vertical Order Traversal Of A Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)
- [Bottom View Of Binary Tree](https://www.geeksforgeeks.org/problems/bottom-view-of-binary-tree/)
- [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/description/)

### Binary Tree Right Side View

DFS (right-first preorder):
```cpp
class Solution {
public:
    vector<int>result;
    void dfs(TreeNode* root,int level){
        if (!root)
            return;
        if (result.size() == level)
            result.emplace_back(root->val);
        dfs(root->right,level+1);
        dfs(root->left,level+1);
    }
    vector<int> rightSideView(TreeNode* root) {
        dfs(root,0);
        return result;
    }
};
```

BFS (level-order):
```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int>result;
        if (!root)
            return result;

        queue<TreeNode*>q;
        q.emplace(root);

        while (!q.empty()){
            int size = q.size();
            for (int i=0;i<size;i++){
                TreeNode* node = q.front();
                q.pop();
                if (node->left) 
                    q.emplace(node->left);
                if (node->right)
                    q.emplace(node->right);
                if (i == size-1)
                    result.emplace_back(node->val);
            }
        }

        return result;
    }
};
```

---

## Constructing a Binary Tree from a Level-Order Array (e.g., [1, 2, 3, 4, 5])

Interpret the array as a heap-style level-order layout:
- At index i: value a[i]
- Left child index = 2i + 1
- Right child index = 2i + 2

Example for [1,2,3,4,5]:
```
      1
     / \
    2   3
   / \
  4   5
```

Handling nulls: Use `optional<int>` entries (or a sentinel) to represent missing nodes in the array.

Canonical implementation (C++17+)
```cpp
#include <bits/stdc++.h>
using namespace std;

struct TreeNode {
    int val; TreeNode *left, *right;
    TreeNode(int x): val(x), left(nullptr), right(nullptr) {}
};

TreeNode* buildFromLevelOrder(const vector<optional<int>>& a) {
    if (a.empty() || !a[0].has_value()) return nullptr;
    vector<TreeNode*> nodes(a.size(), nullptr);
    for (size_t i = 0; i < a.size(); ++i) {
        if (a[i].has_value()) nodes[i] = new TreeNode(*a[i]);
    }
    for (size_t i = 0; i < a.size(); ++i) {
        if (!nodes[i]) continue;
        size_t li = 2 * i + 1, ri = 2 * i + 2;
        if (li < a.size()) nodes[i]->left = nodes[li];
        if (ri < a.size()) nodes[i]->right = nodes[ri];
    }
    return nodes[0];
}

int main() {
    vector<optional<int>> a = {1, 2, 3, 4, 5};
    TreeNode* root = buildFromLevelOrder(a);
}
```

#### Practice Problems:
- [Lowest Common Ancestor Of A Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)