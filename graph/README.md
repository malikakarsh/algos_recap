# Graph Representations: Time and Space Complexity

## Table of Contents
- [Adjacency Matrix](#adjacency-matrix)
- [Adjacency List](#adjacency-list)
- [Directed vs Undirected Graphs](#directed-vs-undirected-graphs)
- [Graph Traversal Algorithms (DFS, BFS)](#graph-traversal-algorithms)
- [Cycle Detection in Undirected Graph](#cycle-detection-in-undirected-graph)
- [Bipartite Graph](#bipartite-graph)
- [Cycle Detection in Directed Graph](#cycle-detection-in-directed-graph)
- [Strongly Connected Components (Kosaraju's Algorithm)](#strongly-connected-components-kosarajus-algorithm)
- [Problem: The "Semi-Connected" Graph](#problem-the-semi-connected-graph)
- [Problem: The "Mother Vertex"](#problem-the-mother-vertex)
- [Problem: 2-SAT (2-Satisfiability)](#problem-2-sat-2-satisfiability)
- [Problem: Finding Bridges via Orientation and SCCs](#problem-finding-bridges-via-orientation-and-sccs)
- [Finding Bridges (Tarjan's Algorithm)](#finding-bridges-tarjans-algorithm)
- [Kahn's Algorithm (Topological Sort)](#kahns-algorithm-topological-sort)
- [Shortest Path in Weighted DAG](#shortest-path-in-weighted-dag)
- [BFS vs DFS for Shortest Path](#bfs-vs-dfs-for-shortest-path-unit-weights)
- [Dijkstra's Algorithm](#dijkstras-algorithm-shortest-path-with-weights)
  - [Problem: The Widest Path](#problem-the-widest-path)
  - [Problem: The Most Reliable Communication Path](#problem-the-most-reliable-communication-path)
  - [Problem: Shortest Path on a 0/1 Graph](#problem-shortest-path-on-a-01-graph)
- [Bellman-Ford Algorithm](#correct-algorithm-for-negative-weights)
  - [Problem: Finding a Negative Cycle](#problem-finding-a-negative-cycle)
- [Minimum Spanning Tree (MST)](#minimum-spanning-tree-mst)
  - [Problem: Decreasing the Weight of a Non-Tree Edge](#problem-decreasing-the-weight-of-a-non-tree-edge)
  - [Problem: Increasing the Weight of a Tree Edge](#problem-increasing-the-weight-of-a-tree-edge)
  - [Problem: Adding a New Vertex z](#problem-adding-a-new-vertex-z)
  - [Problem: MST for Extremely Sparse Graph](#problem-mst-for-extremely-sparse-graph-ev3)
  - [Bottleneck MST: The Decision Problem](#bottleneck-mst-the-decision-problem)
  - [Bottleneck MST: The Optimization Problem](#bottleneck-mst-the-optimization-problem)
  - [Problem: The Second Best Minimum Spanning Tree](#problem-the-second-best-minimum-spanning-tree)
  - [Problem: The Most Vital Edge](#problem-the-most-vital-edge)
- [Disjoint Set Union (DSU)](#disjoint-set-union-union-find)


## Adjacency Matrix

An adjacency matrix is a 2D array where `matrix[i][j]` indicates whether there's an edge between vertex `i` and vertex `j`.

### Time Complexity
- **Add Edge**: O(1) - Direct array access
- **Remove Edge**: O(1) - Direct array access
- **Check if Edge Exists**: O(1) - Direct array access
- **Get All Adjacent Vertices**: O(V) - Must scan entire row
- **Add Vertex**: O(V²) - Need to resize matrix
- **Remove Vertex**: O(V²) - Need to resize matrix

### Space Complexity
- **Overall**: O(V²) where V is the number of vertices
- Uses V² space regardless of the actual number of edges
- Efficient for dense graphs (many edges)
- Wasteful for sparse graphs (few edges)

## Adjacency List

An adjacency list stores a list/array of neighbors for each vertex.

### Time Complexity
- **Add Edge**: O(1) - Append to list (amortized for dynamic arrays)
- **Remove Edge**: O(degree) - Need to find and remove from list
- **Check if Edge Exists**: O(degree) - Need to search through neighbor list
- **Get All Adjacent Vertices**: O(degree) - Direct access to neighbor list
- **Add Vertex**: O(1) - Add new empty list
- **Remove Vertex**: O(V + E) - Remove vertex and all edges pointing to it

### Space Complexity
- **Overall**: O(V + E) where V is vertices and E is edges
- Uses space proportional to actual number of edges
- Efficient for sparse graphs
- May have overhead for storing list pointers

## Comparison Summary

| Operation | Adjacency Matrix | Adjacency List |
|-----------|------------------|----------------|
| Space | O(V²) | O(V + E) |
| Add Edge | O(1) | O(1) |
| Remove Edge | O(1) | O(degree) |
| Edge Exists? | O(1) | O(degree) |
| Get Neighbors | O(V) | O(degree) |
| Add Vertex | O(V²) | O(1) |

## When to Use Which?

**Use Adjacency Matrix when:**
- Graph is dense (many edges)
- Need frequent edge existence checks
- Graph size is small and fixed
- Need to support edge weights with direct access

**Use Adjacency List when:**
- Graph is sparse (few edges)
- Need to frequently iterate over neighbors
- Graph size can change dynamically
- Memory efficiency is important

Note: `degree` refers to the number of neighbors of a vertex, which in the worst case can be O(V).

## Directed vs Undirected Graphs

### Directed Graph
- **Edges have direction**: A → B (one-way)
- **Maximum edges**: V(V-1) = O(V²)
- **Time complexity** (traversal): O(V + E)
- **Space complexity**: 
  - Adjacency Matrix: O(V²)
  - Adjacency List: O(V + E)

### Undirected Graph  
- **Edges are bidirectional**: A ↔ B (two-way)
- **Maximum edges**: V(V-1)/2 = O(V²)
- **Time complexity** (traversal): O(V + E)
- **Space complexity**:
  - Adjacency Matrix: O(V²)
  - Adjacency List: O(V + E), but each edge stored twice

**Note**: For undirected graphs, each edge (u,v) appears twice in adjacency list: once in u's list and once in v's list.

## Graph Traversal Algorithms

### Depth-First Search (DFS)

```cpp
void dfs(vector<vector<int>>& adj, vector<bool>& visited, int node) {
    visited[node] = true;
    
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(adj, visited, neighbor);
        }
    }
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V) - for recursion stack and visited array

### Breadth-First Search (BFS)

```cpp
void bfs(vector<vector<int>>& adj, int start) {
    vector<bool> visited(adj.size(), false);
    queue<int> q;
    
    visited[start] = true;
    q.push(start);
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        for (int neighbor : adj[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V) - for queue and visited array

**Practice Problems**
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
- [Flood Fill](https://leetcode.com/problems/flood-fill/)
- [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
- [Magic Squares In Grid](https://leetcode.com/problems/magic-squares-in-grid/)
- [Pythagorean Distance Nodes In A Tree](https://leetcode.com/problems/pythagorean-distance-nodes-in-a-tree/)


## Cycle Detection in Undirected Graph

### DFS Cycle Detection
```cpp
bool dfsCheckCycle(vector<vector<int>>& adjList, vector<bool>& visited, int node, int parent) {
    visited[node] = true;
    
    for (int neighbor : adjList[node]) {
        if (!visited[neighbor]) {
            if (dfsCheckCycle(adjList, visited, neighbor, node))
                return true;
        }
        else if (neighbor != parent) {
            return true;  // Back edge found - cycle detected
        }
    }
    return false;
}
```

### BFS Cycle Detection
```cpp
bool bfsCheckCycle(vector<vector<int>>& adjList, vector<bool>& visited, int start) {
    queue<pair<int, int>> q;  // {node, parent}
    visited[start] = true;
    q.push({start, -1});
    
    while (!q.empty()) {
        int node = q.front().first;
        int parent = q.front().second;
        q.pop();
        
        for (int neighbor : adjList[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push({neighbor, node});
            }
            else if (neighbor != parent) {
                return true;  // Back edge found - cycle detected
            }
        }
    }
    return false;
}
```

**Key Idea**: In undirected graphs, a cycle exists if we find a back edge to a visited node that's not the parent.

**Practice Problems** 
- [Detect Cycle in Undirected Graph](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph)

**Standard Problems**
- [01 Matrix](https://leetcode.com/problems/01-matrix/)
- [Map of Highest Peak](https://leetcode.com/problems/map-of-highest-peak/)
- [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
- [Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/)
- [Number of Distinct Islands](https://www.geeksforgeeks.org/problems/number-of-distinct-islands)
- [Magic Squares In Grid](https://leetcode.com/problems/magic-squares-in-grid/)

## Bipartite Graph

**Definition**: A graph where vertices can be divided into two sets such that no two vertices within the same set are adjacent. All edges connect vertices from different sets.

**Key Property**: A graph is bipartite if and only if it contains no odd-length cycles.

### DFS Approach
```cpp
bool dfsCheck(vector<vector<int>>& adj, vector<int>& color, int node, int c) {
    color[node] = c;
    
    for (int neighbor : adj[node]) {
        if (color[neighbor] == -1) {
            if (!dfsCheck(adj, color, neighbor, 1 - c))
                return false;
        }
        else if (color[neighbor] == color[node]) {
            return false;  // Same color conflict
        }
    }
    return true;
}
```

### BFS Approach
```cpp
bool bfsCheck(vector<vector<int>>& adj, vector<int>& color, int start) {
    queue<int> q;
    color[start] = 0;
    q.push(start);
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        for (int neighbor : adj[node]) {
            if (color[neighbor] == -1) {
                color[neighbor] = 1 - color[node];
                q.push(neighbor);
            }
            else if (color[neighbor] == color[node]) {
                return false;  // Same color conflict
            }
        }
    }
    return true;
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

**Practice Problems**: 
- [Bipartite Graph](https://www.geeksforgeeks.org/problems/bipartite-graph/)

## Cycle Detection in Directed Graph

**Key Idea**: Use DFS with state tracking. States: 0 = unvisited, 1 = visiting (in current path), 2 = visited (completely processed).

### DFS Approach
```cpp
bool dfsCheckCycle(vector<vector<int>>& adj, vector<int>& state, int node) {
    state[node] = 1;  // Visiting
    
    for (int neighbor : adj[node]) {
        if (state[neighbor] == 0) {
            if (dfsCheckCycle(adj, state, neighbor))
                return true;
        }
        else if (state[neighbor] == 1) {
            return true;  // Back edge found - cycle detected
        }
    }
    
    state[node] = 2;  // Visited
    return false;
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V) - for state array and function call stack

**Practice Problems**:
- [Detect Cycle in Directed Graph](https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph)

## Strongly Connected Components (Kosaraju's Algorithm)

**Definition**: In a directed graph, a **Strongly Connected Component (SCC)** is a maximal set of vertices such that for every pair of vertices `u` and `v` in the set, there is a directed path from `u` to `v` and a directed path from `v` to `u`. Determining these components is crucial for understanding the cyclic structure of a graph.

**Use of Kosaraju's Algorithm**:
Kosaraju's algorithm is a classic, efficient (linear time) method to **detect and list all SCCs** in a directed graph.

**Algorithm Steps (Two DFS Passes)**:
1. **First Pass (DFS on G)**: Perform a DFS traversal of the graph. When a recursive DFS call for a vertex finishes (i.e., after visiting all its neighbors), push the vertex onto a stack. This effectively sorts vertices by finishing time.
2. **Transpose Graph (G^T)**: Create the transpose of the graph by reversing the direction of every edge.
3. **Second Pass (DFS on G^T)**: Process vertices from the stack (LIFO). If a vertex has not been visited in this second pass, start a new DFS from it on the transpose graph. All vertices reachable in this DFS traversal belong to the same SCC.

**Why it works**:
Topological sorting properties ensure that by processing vertices in decreasing order of finishing times on the transpose graph, we are guaranteed to identify "sink" components first and work our way backwards, isolating each SCC.

**Complexity**: O(V + E) time, O(V + E) space.

```cpp
// Kosaraju's algorithm: returns list of SCCs (each SCC is a vector of vertices)
vector<vector<int>> kosarajuSCC(const vector<vector<int>>& adj) {
    int V = (int)adj.size();
    vector<int> order; order.reserve(V);
    vector<char> vis(V, 0);

    function<void(int)> dfsOrder = [&](int u) {
        vis[u] = 1;
        for (int v : adj[u]) if (!vis[v]) dfsOrder(v);
        order.push_back(u); // record by finish time
    };

    for (int u = 0; u < V; ++u) if (!vis[u]) dfsOrder(u);

    // Build transpose graph G^T
    vector<vector<int>> radj(V);
    for (int u = 0; u < V; ++u) for (int v : adj[u]) radj[v].push_back(u);

    // Second pass on G^T in reverse finish order
    fill(vis.begin(), vis.end(), 0);
    vector<vector<int>> components;
    function<void(int, vector<int>&)> dfsCollect = [&](int u, vector<int>& comp) {
        vis[u] = 1; comp.push_back(u);
        for (int v : radj[u]) if (!vis[v]) dfsCollect(v, comp);
    };

    for (int i = V - 1; i >= 0; --i) {
        int u = order[i];
        if (!vis[u]) {
            vector<int> comp;
            dfsCollect(u, comp);
            components.push_back(move(comp));
        }
    }
    return components;
}
```

**Practice Problem**:
- [Strongly Connected Components (Kosaraju's Algo)](https://www.geeksforgeeks.org/problems/strongly-connected-components-kosarajus-algo/1)

## Problem: The "Semi-Connected" Graph

**Problem Statement**: Given a directed graph $G=(V,E)$, determine if it is semi-connected. A graph is semi-connected if for every pair of vertices $(u,v)$, there is either a path $u \to v$ OR a path $v \to u$.

### The Solution

1. **Condense Graph**: Use Kosaraju's or Tarjan's algorithm to build the Component Graph (DAG) where each node is an SCC.
2. **Topological Sort**: Get the topological ordering of these components: $C_1, C_2, \dots, C_k$.
3. **Linear Chain Check**: Verify if the Component Graph forms a Linear Chain (Hamiltonian Path).
   - Iterate through the sorted components from $i=0$ to $k-2$.
   - Check if there is a direct edge $C_i \to C_{i+1}$ in the component graph.
   - If any link is missing, return `False`.

**Result**: If the loop finishes successfully, return `True`.

**Key Insight**: Checking if the "root" reaches everything is insufficient (e.g., a "V" shape graph fails). You must verify that every component reaches the immediately next component in the topological sort to ensure no "branches" exist.

## Problem: The "Mother Vertex"

**Problem Statement**: Given a directed graph $G=(V,E)$, find a Mother Vertex $v$ such that all other vertices in $G$ are reachable from $v$. If none exist, report it.

### The Solution

1. **Find Candidate**: Run a standard DFS (or Kosaraju's first pass) on the whole graph and track the finish times.
   - The vertex that finishes last (at the top of the stack) is the only possible candidate. Let's call it $v_{cand}$.
2. **Verify Candidate**: Run a second DFS/BFS starting only from $v_{cand}$.
3. **Count Nodes**: Count how many unique nodes were visited in this second pass.

**Result**:
- If count == $V$ (all nodes visited), then $v_{cand}$ is a Mother Vertex.
- Otherwise, no Mother Vertex exists.

**Key Insight**: A Mother Vertex can only exist in a Source SCC (in-degree 0 in the Component DAG). If there are multiple Source SCCs, no Mother Vertex exists. The "last finished node" logic efficiently identifies a node from a Source SCC without needing to build the full component graph.

## Problem: 2-SAT (2-Satisfiability)

**Problem Statement**: Given a boolean formula in 2-CNF form (clauses with 2 literals, e.g., $x_1 \lor \neg x_2$), determine if there is a valid assignment of True/False to variables such that the formula evaluates to True.

### The Solution

1. **Build Graph**:
   - **Nodes**: Create two nodes for each variable $x_i$: one for $x_i$ (True) and one for $\neg x_i$ (False). (Total $2N$ nodes).
   - **Edges**: For every clause $(A \lor B)$, add two directed implication edges:
     - $\neg A \to B$ ("If not A, then B")
     - $\neg B \to A$ ("If not B, then A")

2. **Find SCCs**: Run Tarjan's or Kosaraju's to find the SCC ID for every node.

3. **Check for Contradiction**:
   - Iterate through every variable $x_i$.
   - Check if `SCC(x_i) == SCC(NOT x_i)`.
   - If they are in the same component, it implies a logical contradiction ($x_i \leftrightarrow \neg x_i$).

**Result**:
- If any variable conflicts, return "Unsatisfiable".
- If no conflicts, return "Satisfiable".

**Key Insight**: The clause $(A \lor B)$ is logically equivalent to the implications $(\neg A \to B)$ and $(\neg B \to A)$. A contradiction occurs only if a variable and its negation force each other (i.e., they end up in the same cycle/SCC).

## Problem: Finding Bridges via Orientation and SCCs

**Problem Description**:
Let $G = (V, E)$ be a connected undirected graph. An edge $e = \{u, v\}$ is a **bridge** if removing $e$ disconnects $G$. Note that $\{u, v\}$ is a bridge if and only if there is exactly one $u-v$ path in $G$.

Our goal is to design an $O(n + m)$-time algorithm to output all bridges of $G$. The plan is to **orient** the edges of $G$ to obtain a directed graph $\vec{G}$ with the property that every non-bridge edge $xy$ has at least a path $x \leadsto y$ and a path $y \leadsto x$ in $\vec{G}$. Consequently, $x$ and $y$ lie in the same Strongly Connected Component (SCC) in the directed graph, while the endpoints of each bridge lie in **different** SCCs.

**(a) Describe how to orient every undirected edge.**
**(b) Using the above, give a complete $O(n + m)$-time algorithm to output all bridges.**

### Solution (C++)

The algorithm follows these steps:
1.  **Orientation**: Perform a DFS on the undirected graph. For every edge $(u, v)$:
    -   If $v$ is a child of $u$ in the DFS tree, orient $u \to v$.
    -   If $v$ is an ancestor of $u$ (back edge), orient $u \to v$.
    -   This ensures all cycles in the undirected graph become strong components in the directed graph.
2.  **SCC Detection**: Run Kosaraju's Algorithm on the newly oriented graph $\vec{G}$.
3.  **Bridge Identification**: Any edge $u \to v$ in $\vec{G}$ where $SCC[u] \neq SCC[v]$ corresponds to a bridge in the original graph.

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <algorithm>

using namespace std;

// Step 1: DFS to orient edges (Build Directed Graph)
// - Tree edges: u -> v
// - Back edges: u -> v (u connects back to ancestor v)
void orientEdgesDFS(const vector<vector<int>>& undirGraph, vector<vector<int>>& directedGraph, 
                    vector<int>& visited, int src, int parent) {
    visited[src] = 1; // Mark as visited
    for (int neighbor : undirGraph[src]) {
        if (neighbor == parent) continue; // Skip the edge we came from
        
        if (visited[neighbor]) {
            // If already visited, it's a back edge/ancestor.
            // Note: In undirected DFS, if visited, it must be an ancestor (or processed).
            // We verify visited[neighbor] == 1 to ensure it's an active ancestor if we want strictly back edges,
            // but for simple orientation, if it's visited and not parent, we orient u -> v.
            // Check if edge already added? undirected graph has u-v and v-u.
            // We only add u->v if we process u first. 
            // The simple logic: if neighbor visited, orient u->v.
            // But wait, if v visited u (parent), we skipped.
            // If v implies a cycle, we orient u->v.
            // To avoid adding duplicates or reverse edges for tree edges, we check visited.
            if (visited[neighbor] == 1) { 
                 directedGraph[src].push_back(neighbor);
            }
        } else {
            // Tree edge
            directedGraph[src].push_back(neighbor);
            orientEdgesDFS(undirGraph, directedGraph, visited, neighbor, src);
        }
    }
    visited[src] = 2; // Mark as finished
}

// Step 2 & 3: Kosaraju's Algorithm Helper - Pass 1 (Fill Stack)
void kosarajuPass1(const vector<vector<int>>& graph, vector<bool>& visited, stack<int>& order, int u) {
    visited[u] = true;
    for (int v : graph[u]) {
        if (!visited[v]) kosarajuPass1(graph, visited, order, v);
    }
    order.push(u);
}

// Step 2 & 3: Kosaraju's Algorithm Helper - Pass 2 (Find SCCs)
void kosarajuPass2(const vector<vector<int>>& revGraph, vector<bool>& visited, vector<int>& sccIds, int id, int u) {
    visited[u] = true;
    sccIds[u] = id;
    for (int v : revGraph[u]) {
        if (!visited[v]) kosarajuPass2(revGraph, visited, sccIds, id, v);
    }
}

void findBridges(int V, const vector<vector<int>>& undirGraph) {
    // 1. Orient Edges
    // 0: unvisited, 1: visiting, 2: visited
    vector<vector<int>> directedGraph(V);
    vector<int> visitedState(V, 0); 
    orientEdgesDFS(undirGraph, directedGraph, visitedState, 0, -1);

    // 2. Find SCCs on directedGraph
    //    a. Pass 1: Get finish order
    stack<int> order;
    vector<bool> visited(V, false);
    for (int i = 0; i < V; ++i) {
        if (!visited[i]) kosarajuPass1(directedGraph, visited, order, i);
    }

    //    b. Build Transpose Graph
    vector<vector<int>> revGraph(V);
    for (int u = 0; u < V; ++u) {
        for (int v : directedGraph[u]) {
            revGraph[v].push_back(u);
        }
    }

    //    c. Pass 2: Process in reverse finish order
    fill(visited.begin(), visited.end(), false);
    vector<int> sccIds(V, -1);
    int sccCount = 0;
    while (!order.empty()) {
        int u = order.top();
        order.pop();
        if (!visited[u]) {
            kosarajuPass2(revGraph, visited, sccIds, sccCount++, u);
        }
    }

    // 3. Find Bridges
    // Iterate over original edges (or directed edges)
    // If endpoints are in different SCCs, it's a bridge.
    cout << "Bridges found:\n";
    for (int u = 0; u < V; ++u) {
        for (int v : directedGraph[u]) {
            if (sccIds[u] != sccIds[v]) {
                cout << u << " -- " << v << "\n";
            }
        }
    }
}

int main() {
    int V = 6;
    vector<vector<int>> undirGraph(V);
    vector<pair<int, int>> edges = {{0,1}, {1,2}, {2,0}, {2,3}, {3,5}, {5,4}, {4,3}};
    
    for (auto& edge : edges) {
        undirGraph[edge.first].push_back(edge.second);
        undirGraph[edge.second].push_back(edge.first);
    }

    findBridges(V, undirGraph);
    return 0;
}
```

## Finding Bridges (Tarjan's Algorithm)

**Concept**:
Tarjan's bridge-finding algorithm is based on DFS traversal. It uses two arrays:
1. `tin[u]`: Time of insertion (discovery time) of node `u`.
2. `low[u]`: Lowest discovery time reachable from `u` (represented by `tin`) in the DFS tree, possibly using a back-edge (an edge to an ancestor).

**Bridge Condition**:
An edge `u-v` (where `v` is a child of `u` in DFS tree) is a bridge if and only if `low[v] > tin[u]`.
This means there is no back-edge from `v` or any of its descendants to `u` or any of its ancestors. If `low[v] <= tin[u]`, it implies there's a path from `v` back to `u` or above, forming a cycle, so removing `u-v` wouldn't disconnect the component.

**Complexity**:
- **Time**: O(V + E) - Standard DFS traversal.
- **Space**: O(V + E) - Graph storage + recursion stack + arrays.

### Solution (C++)

```cpp
// Finds all critical connections (bridges) in an undirected graph
// Time Complexity: O(V + E)
// Space Complexity: O(V + E)
class Solution {
public:
    int timer;
    vector<int> tin, low;
    vector<vector<int>> bridges;

    void dfs(int node, int parent, const vector<vector<int>>& graph) {
        tin[node] = low[node] = timer++;
        
        for (int neighbor : graph[node]) {
            if (neighbor == parent) continue; // Skip edge to parent
            
            if (tin[neighbor] == -1) { // If not visited
                dfs(neighbor, node, graph);
                
                // Update low-link value based on child's low-link
                low[node] = min(low[node], low[neighbor]);
                
                // Bridge condition: if no back-edge from neighbor (or its subtree) to node (or ancestors)
                if (low[neighbor] > tin[node]) {
                    bridges.push_back({node, neighbor});
                }
            } else {
                // Back-edge: update low-link based on ancestor's discovery time
                low[node] = min(low[node], tin[neighbor]);
            }
        }
    }

    vector<vector<int>> findBridges(int n, vector<vector<int>>& connections) {
        vector<vector<int>> graph(n);
        for (const auto& conn : connections) {
            graph[conn[0]].push_back(conn[1]);
            graph[conn[1]].push_back(conn[0]);
        }
        
        timer = 0;
        tin.assign(n, -1);
        low.resize(n);
        bridges.clear();
        
        for (int i = 0; i < n; ++i) {
            if (tin[i] == -1) {
                dfs(i, -1, graph);
            }
        }
        return bridges;
    }
};
```
### Practice Problems:
- [Critical Connections In A Network](https://leetcode.com/problems/critical-connections-in-a-network/)

## Kahn's Algorithm (Topological Sort)

**Definition**: Topological sorting of a directed acyclic graph (DAG) is a linear ordering of vertices such that for every directed edge (u, v), vertex u comes before v in the ordering.

**Key Idea**: Repeatedly remove vertices with no incoming edges (indegree = 0) and update indegrees of their neighbors.

### BFS Implementation (Kahn's Algorithm)
```cpp
vector<int> topologicalSort(vector<vector<int>>& adj, int V) {
    vector<int> indegree(V, 0);
    
    // Calculate indegree for all vertices
    for (int i = 0; i < V; i++) {
        for (int neighbor : adj[i]) {
            indegree[neighbor]++;
        }
    }
    
    queue<int> q;
    // Add all vertices with indegree 0
    for (int i = 0; i < V; i++) {
        if (indegree[i] == 0) {
            q.push(i);
        }
    }
    
    vector<int> topoOrder;
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        topoOrder.push_back(node);
        
        // Reduce indegree of neighbors
        for (int neighbor : adj[node]) {
            indegree[neighbor]--;
            if (indegree[neighbor] == 0) {
                q.push(neighbor);
            }
        }
    }
    
    return topoOrder;
}
```

**Applications**: Course scheduling, build systems, dependency resolution

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V) - for indegree array and queue

**Cycle Detection**: If `topoOrder.size() != V`, then graph has a cycle.

### DFS Implementation (Alternative Approach)
```cpp
void dfsTopoSort(vector<vector<int>>& adj, vector<bool>& visited, stack<int>& topoStack, int node) {
    visited[node] = true;
    
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfsTopoSort(adj, visited, topoStack, neighbor);
        }
    }
    
    topoStack.push(node);  // Add to stack after visiting all neighbors
}

vector<int> topologicalSortDFS(vector<vector<int>>& adj, int V) {
    vector<bool> visited(V, false);
    stack<int> topoStack;
    
    for (int i = 0; i < V; i++) {
        if (!visited[i]) {
            dfsTopoSort(adj, visited, topoStack, i);
        }
    }
    
    vector<int> topoOrder;
    while (!topoStack.empty()) {
        topoOrder.push_back(topoStack.top());
        topoStack.pop();
    }
    
    return topoOrder;
}
```

**Key Idea**: Add vertex to result AFTER visiting all its dependencies (neighbors).

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V) - for visited array, stack, and recursion stack

### Why Topo Sort Works Only For DAGs?

**Directed Graphs with Cycles**: 
- Cycles create circular dependencies (A → B → C → A)
- No vertex in the cycle can come "before" all others
- Algorithm gets stuck when all remaining vertices have indegree > 0

**Undirected Graphs**:
- Every edge creates a bidirectional dependency (A ↔ B)
- Both vertices depend on each other simultaneously
- No meaningful "ordering" can satisfy both directions

**Practice Problems**: 
- [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)
- [Course Schedule](https://leetcode.com/problems/course-schedule/)
- [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
- [Alien Dictionary](https://www.geeksforgeeks.org/problems/alien-dictionary)


## Shortest Path in Weighted DAG

**Key Idea**: For DAGs with weights, topological sort ensures we process vertices in correct order, avoiding unnecessary recursion calls and guaranteeing shortest paths.

### Why Topological Sort is Essential?
- **Optimal processing order**: Process vertices before their dependencies
- **Single pass**: Each vertex is processed exactly once
- **No cycles**: Eliminates infinite loops that can occur with negative weights
- **Efficiency**: O(V + E) instead of O(VE) for Bellman-Ford

### Implementation
```cpp
vector<int> shortestPathDAG(vector<vector<pair<int, int>>>& adj, int src, int V) {
    // Step 1: Get topological order
    vector<int> topoOrder = topologicalSort(adj, V);
    
    // Step 2: Initialize distances
    vector<int> dist(V, INT_MAX);
    dist[src] = 0;
    
    // Step 3: Process vertices in topological order
    for (int node : topoOrder) {
        if (dist[node] != INT_MAX) {
            for (auto [neighbor, weight] : adj[node]) {
                if (dist[node] + weight < dist[neighbor]) {
                    dist[neighbor] = dist[node] + weight;
                }
            }
        }
    }
    
    // Optimization: Skip nodes before source in topological order
    // They won't be reachable anyway, so no need to process them
    
    return dist;
}
```

**Time Complexity**: O(V + E)  
**Space Complexity**: O(V)

**Optimization**: Skip nodes before source in topological order - they won't be reachable anyway.

**Practice Problems**:
- [Shortest path in Directed Acyclic Graph](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph)
- [All Paths From Source To Target](https://leetcode.com/problems/all-paths-from-source-to-target/)

## BFS vs DFS for Shortest Path (Unit Weights)

### Why BFS Works for Shortest Path?
- **Level-by-level exploration**: BFS visits vertices in order of their distance from source
- **First visit is shortest**: When BFS first visits a vertex, it's guaranteed to be the shortest path
- **Unit weights**: All edges have same weight, so distance = number of edges
- **Optimal complexity**: O(V + E) - visits each vertex/edge at most once

### Why DFS Fails (Exceeds Time Limit)?
- **Depth-first exploration**: DFS goes deep into one path before exploring others
- **May revisit vertices**: Can visit same vertex multiple times through different paths
- **No guarantee of shortest**: First visit might not be the shortest path
- **Exponential complexity**: Can explore exponentially many paths

### Example
```
Graph: A -- B -- C
       |    |
       D -- E

BFS from A: A(0) → B(1), D(1) → C(2), E(2)  // Optimal
DFS from A: A → B → C → E → D → B → C...     // May revisit
```

**Practice Problems**:
- [Shortest path in Undirected Graph with Unit Distance](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph-having-unit-distance/)

**Standard Problems**:
- [Word Ladder](https://leetcode.com/problems/word-ladder/)
- [Word Ladder ii](https://leetcode.com/problems/word-ladder-ii/)

## Dijkstra's Algorithm (Shortest Path with Weights)

**Definition**: Finds shortest paths from a source vertex to all other vertices in a weighted graph with non-negative edge weights.

**Key Idea**: Greedily select the unvisited vertex with minimum distance and relax all its neighbors.

### 1. Using Priority Queue (Most Efficient)
```cpp
vector<int> dijkstra(vector<vector<pair<int, int>>>& adj, int src, int V) {
    vector<int> dist(V, INT_MAX);
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    
    dist[src] = 0;
    pq.push({0, src});  // {distance, vertex}
    
    while (!pq.empty()) {
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();
        
        if (d > dist[u]) continue;  // Skip outdated entries
        
        for (auto [v, weight] : adj[u]) {
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    
    return dist;
}
```
**Time Complexity**: O((V + E) log V)  
**Space Complexity**: O(V)

### 2. Using Simple Queue (Inefficient)
```cpp
vector<int> dijkstraQueue(vector<vector<pair<int, int>>>& adj, int src, int V) {
    vector<int> dist(V, INT_MAX);
    vector<bool> visited(V, false);
    queue<int> q;
    
    dist[src] = 0;
    q.push(src);
    
    while (!q.empty()) {
        // Find minimum distance unvisited vertex in queue
        int u = -1, minDist = INT_MAX;
        queue<int> temp;
        
        while (!q.empty()) {
            int curr = q.front();
            q.pop();
            if (!visited[curr] && dist[curr] < minDist) {
                minDist = dist[curr];
                u = curr;
            }
            temp.push(curr);
        }
        
        q = temp;  // Restore queue
        if (u == -1) break;
        
        visited[u] = true;
        
        for (auto [v, weight] : adj[u]) {
            if (!visited[v] && dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                q.push(v);
            }
        }
    }
    
    return dist;
}
```
**Time Complexity**: O(V² + E)  
**Space Complexity**: O(V)

### 3. Using Hash Map with Set (Alternative)
```cpp
vector<int> dijkstraSet(vector<vector<pair<int, int>>>& adj, int src, int V) {
    vector<int> dist(V, INT_MAX);
    set<pair<int, int>> activeVertices;  // {distance, vertex}
    
    dist[src] = 0;
    activeVertices.insert({0, src});
    
    while (!activeVertices.empty()) {
        int u = activeVertices.begin()->second;
        activeVertices.erase(activeVertices.begin());
        
        for (auto [v, weight] : adj[u]) {
            if (dist[u] + weight < dist[v]) {
                activeVertices.erase({dist[v], v});  // Remove old entry
                dist[v] = dist[u] + weight;
                activeVertices.insert({dist[v], v});  // Insert new entry
            }
        }
    }
    
    return dist;
}
```
**Time Complexity**: O((V + E) log V)  
**Space Complexity**: O(V)

## Efficiency Comparison

| Implementation | Time Complexity | Space Complexity | Efficiency |
|---------------|----------------|------------------|------------|
| Priority Queue | O((V + E) log V) | O(V) | **Best** |
| Set-based | O((V + E) log V) | O(V) | Good |
| Simple Queue | O(V² + E) | O(V) | **Worst** |

### Practice Problems:
- [Minimum Time to Visit Disappearing Nodes](https://leetcode.com/problems/minimum-time-to-visit-disappearing-nodes/)
- [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
- [Path With Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)

### Why Priority Queue is Most Efficient?

1. **Optimal Time Complexity**: O((V + E) log V) vs O(V² + E) for simple queue
2. **No Manual Search**: Priority queue automatically gives minimum distance vertex
3. **Handles Duplicates**: Skip outdated entries efficiently
4. **Standard Library**: Optimized implementation

### When to Use Each?

- **Priority Queue**: Default choice for most cases
- **Set-based**: When you need to remove specific entries frequently
- **Simple Queue**: Only for educational purposes (inefficient)

**Applications**: GPS navigation, network routing, flight connections

### Why Dijkstra Fails with Negative Weights?

**Short Answer**: Dijkstra's algorithm fails if the graph has **any** negative weight edge. It does not need a negative cycle to fail; a single negative edge is enough to produce an incorrect result.

**The Misconception**: 
Many people think Dijkstra only fails if there is a negative *cycle* (a loop with total negative weight). This is incorrect. 
- **Negative Cycle**: Causes the algorithm to loop infinitely (or until integer overflow) because it keeps finding "shorter" paths.
- **Negative Edge (No Cycle)**: Causes the algorithm to return a **wrong answer** (suboptimal path) because it violates the "Greedy Choice Property".

### Why It Fails (The Greedy Flaw)

Dijkstra's algorithm assumes that **once a node is "visited" (processed from the priority queue), its shortest distance is finalized and will never change.**

With non-negative weights, this is true: you can't reach a node, go further, and come back with a shorter total distance because adding positive numbers always increases the total.

**With negative edges, this assumption breaks.** You might reach a node `u`, mark it as revisited, and process its neighbors. Later, you might find a "longer" path to a different node `v` that leads to a negative edge aiming back at `u`, reducing `u`'s distance *after* it was supposed to be finalized. Dijkstra does not account for this.

### Example: Negative Edge (No Cycle) -> Wrong Answer

Consider this directed graph:

```
      (2)
  A ------> B
  |        /
(5)|      / (-10)
  v     v
  C <-----
```

Edges:
- A -> B (weight 2)
- A -> C (weight 5)
- B -> C (weight -10)

**Goal**: Find shortest path from A to C.
**Correct Answer**: A -> B -> C = 2 + (-10) = -8.

**Dijkstra's Execution**:
1. **Start**: `dist[A] = 0`, `dist[B] = ∞`, `dist[C] = ∞`. PQ: `[(0, A)]`
2. **Pop A** (dist 0):
   - Relax B: `dist[B] = 2`. PQ: `[(2, B)]`
   - Relax C: `dist[C] = 5`. PQ: `[(2, B), (5, C)]`
3. **Pop B** (dist 2) **(Greedy Choice)**:
   - Relax C: `2 + (-10) = -8`. 
   - Weight is -8. `dist[C]` was 5. New dist is -8. 
   - **Crucial Step**: In standard Dijkstra (using a `visited` set), if B is processed, we might have successfully updated C. **BUT**, consider a slightly more complex case where the order matters significantly or if we use the "visited" array version.

Let's look at a case where the greedy choice explicitly fails to explore a path because it looks "expensive" initially but becomes cheap later.

**Standard Counter-Example**:
```
       2
  A --------> B
  |           |
  | 5         | -10
  v           v
  D <-------- C
```
Path A->D cost 5.
Path A->B->C->D cost 2 + ?? 
Actually, the simplest failure is:
```
    A
   / \
  2   5
 /     \
B       C
 \     /
 -5   -2
  \   /
    D
```
The standard example where Dijkstra fails usually involves a node being "finalized" before a better path to it is found using a negative edge.

**Clear Counter-Example:**
Graph: Use 3 nodes: Source S, Node A, Node B.
Edges: S->A (5), S->B (7), A->B (-4).
Shortest Path S->B: S->A->B = 5 + (-4) = 1.

**Dijkstra Trace**:
1. PQ: `[(0, S)]`
2. Pop S. Neighbors: A(5), B(7).
   - `dist[A]=5`, `dist[B]=7`.
   - PQ: `[(5, A), (7, B)]`
3. **Pop A** (dist 5): 
   - Relax B: `5 + (-4) = 1`. `dist[B]=1`.
   - PQ: `[(1, B), (7, B)]` (Duplicate or update depending on impl)
   
*Wait, Dijkstra works here?* Yes, the version using Priority Queue often "fixes" this by re-inserting nodes. **However**, the time complexity guarantee is lost (it becomes exponential in worst case) or it fails if we strictly don't re-process "visited" nodes.

**Where it REALLY fails (Strict Dijkstra w/ Visited Array)**:
Many implementations use a `visited` boolean array to never process a node twice to guarantee O(E log V).
1. S->A(2), S->B(2).
2. From B, edge to C with weight 1. (Path S-B-C cost 3).
3. From A, edge to B with weight -2. (Path S-A-B cost 0).
   
If we process B first (tie-break), B is marked visited. `dist[B]=2`.
Later we process A. We find edge A->B (-2). Total to B is 0. 
If we obey `if (visited[B]) continue;`, we **miss the update**. `dist[B]` remains 2 (Wrong!). 
Correct `dist[B]` is 0.

**Conclusion**:
- Dijkstra with `visited` array: Returns **WRONG ANSWER**.
- Dijkstra with Priority Queue (no visited check): Might work but has **EXPONENTIAL** time complexity in worst case with negative edges (re-processing nodes many times).
- **Just use Bellman-Ford**.

### Infinite Loop (Negative Cycle)
If the graph acts like:
A --(-1)--> B --(-1)--> A
Dijkstra (without visited check) will keep going:
A(0) -> B(-1) -> A(-2) -> B(-3) -> ... infinitely.

### Problem: The Widest Path

**Scenario**: You are given a directed, weighted graph $G=(V,E)$, where the weight $w(u,v)$ of an edge represents the capacity or width of that connection (e.g., the max weight a bridge can support, or bandwidth of a network link). You need to send a single item from a source vertex $s$ to a target vertex $t$.

**Goal**: Find a simple path from $s$ to $t$ such that the minimum edge weight on that path is maximized. Mathematically, we want to find a path $P$ that maximizes:

$$Bottleneck(P) = \min_{e \in P} (w(e))$$

#### Solution 1: Modified Dijkstra (The Greedy Approach)

This is the preferred method for finding the path between a specific pair $(s,t)$ because it can stop early once the target is finalized.

**The Concept**: Just as standard Dijkstra greedily picks the closest node to extend the shortest path, "Widest Path Dijkstra" greedily picks the widest reachable node to extend the highest-capacity path.

**The Algorithm**:

1. **State Initialization**:
   - Create an array `width[]` to store the maximum bottleneck capacity found so far to each node.
   - Initialize `width[s] =` $\infty$ (The source has no restriction).
   - Initialize all other `width[v] = -1` (or 0).

2. **Priority Queue**:
   - Use a **Max-Heap** (not Min-Heap). We want to process high-capacity paths first.
   - Push `(infinity, s)` to the queue.

3. **Process Loop**:
   - While the queue is not empty:
     - Pop the node `u` with the largest capacity `cap`.
     - **Early Exit**: If `u == t`, return `cap`.
     - **Optimization**: If `cap < width[u]`, skip (we found a better way to `u` previously).
     - **Relax Neighbors**: For each neighbor `v` with edge weight `w`:
       - Calculate the bottleneck of extending the path to `v`: `new_width = min(cap, w)`
       - **Check**: Is this path wider than the previous best path to `v`? `if (new_width > width[v])`
       - **Update**: `width[v] = new_width`. Push `(new_width, v)` to the queue.

**Time Complexity**: $O(E \log V)$. (Same as standard Dijkstra).

#### Solution 2: Maximum Spanning Tree (Kruskal’s Variant)

This method is conceptually simpler and relies on the property that the path between any two nodes in a Maximum Spanning Tree (MaxST) is guaranteed to be the widest possible path.

**The Concept**: If we greedily add the widest edges in the entire graph (checking for cycles), we implicitly connect components with the "strongest" possible links first. The moment $s$ and $t$ become connected, the path formed inside this tree is the optimal bottleneck path.

**The Algorithm**:

1. **Sort Edges**:
   - Sort all edges $E$ in descending order of their weights.

2. **Initialize DSU (Disjoint Set Union)**:
   - Create a DSU structure where every node is its own parent.

3. **Iterate Sorted Edges**:
   - Loop through edges $(u,v)$ with weight $w$ from largest to smallest.
   - Check `Find(u)` and `Find(v)`.
   - If they are in different sets:
     - `Union(u, v)`.
     - **Check Connectivity**: Immediately check if `(Find(s) == Find(t))`.
     - If `True`: The current edge weight $w$ is the answer. Stop immediately.

**Why this works**: Since we process edges from largest to smallest, the edge that finally merges the component containing $s$ and the component containing $t$ must be the "smallest" edge on the path formed so far—and thus the bottleneck. Since any alternative path would rely on edges we haven't processed yet (which are even smaller), this must be optimal.

**Time Complexity**: $O(E \log E)$ or $O(E \log V)$ (Dominated by sorting).

### Problem: The Most Reliable Communication Path

**Given**: A directed graph $G=(V,E)$ representing a communication network.
- For every edge $(u,v) \in E$, there is an associated value $r(u,v)$ where $0 < r(u,v) \le 1$.
- This value $r(u,v)$ represents the reliability (probability of success) of the channel from $u$ to $v$.
- The failures of edges are independent events.

The reliability of a path $P = \langle v_0, v_1, \dots, v_k \rangle$ is defined as the product of the reliabilities of its edges:
$$R(P) = \prod_{i=1}^{k} r(v_{i-1}, v_i)$$

**Goal**: Give an efficient algorithm to find the **Most Reliable Path** (the path with the maximum product of edge reliabilities) between two given vertices $s$ and $t$. You must model this as a Shortest Path Problem by specifying:
1. The **Graph** structure.
2. The **Edge Weights**.
3. The **Algorithm** used.

**The Mathematical Transformation**

Standard shortest path algorithms (like Dijkstra) are designed to minimize the sum of edge weights. Our problem asks to maximize the product of reliabilities. We need to transform the math to fit the tool.

1. **Step 1: Product to Sum (Logarithms)**
   - We want to maximize the product $P = r_1 \times r_2 \times \dots \times r_k$.
   - Since the logarithm function is monotonically increasing, maximizing $P$ is equivalent to maximizing $\ln(P)$.
   $$ \ln(P) = \ln(r_1) + \ln(r_2) + \dots + \ln(r_k) $$

2. **Step 2: Maximize to Minimize (Negation)**
   - Since $0 < r \le 1$, the logarithm $\ln(r)$ will always be negative (or 0).
   - Maximizing a sum of negative numbers (e.g., trying to get close to 0) is equivalent to minimizing the sum of their absolute values.
   - We define the cost as $-\ln(r)$. Since $\ln(r) \le 0$, the value $-\ln(r)$ is always non-negative ($\ge 0$).
   - This allows us to use standard minimization algorithms.

**The Solution**

1. **The Graph**: Construct a directed graph $G'=(V,E)$ that has the exact same vertices and directed edges as the input graph $G$.
2. **The Edge Weights**: For every edge $(u,v)$ in the graph with reliability $r(u,v)$, assign a new weight $w(u,v)$ defined as:
   $$w(u,v) = -\ln(r(u,v))$$
   (Note: Since $0 < r \le 1$, we are guaranteed that $w \ge 0$.)
3. **The Algorithm**: Run **Dijkstra’s Algorithm** on $G'$ starting from source $s$.

**Why Dijkstra?**
- We successfully transformed the problem into finding the path with the minimum sum of weights.
- Because all transformed weights $w(u,v)$ are non-negative, Dijkstra is the optimal choice.
- It is more efficient than Bellman-Ford ($O(E \log V)$ vs $O(VE)$).

**Complexity Analysis**
- **Transformation**: Iterating through all edges to compute logs takes $O(E)$.
- **Dijkstra**: Running the algorithm using a Min-Priority Queue takes $O(E \log V)$.
- **Total Time Complexity**: $O(E \log V)$.

**Example Walkthrough**
- Edge A: $r=1.0$ (Perfect) $\to w = -\ln(1) = 0$.
- Edge B: $r=0.5$ (Risky) $\to w = -\ln(0.5) \approx 0.69$.
- Edge C: $r=0.1$ (Bad) $\to w = -\ln(0.1) \approx 2.30$.

Dijkstra will naturally prefer Edge A (Cost 0) over Edge B (Cost 0.69), which corresponds to preferring 100% reliability over 50%. The model works perfectly.



### Problem: Shortest Path on a 0/1 Graph

**Given**: A directed, weighted graph $G=(V,E)$ where every edge weight $w(u,v)$ is restricted to be either 0 or 1. You are given a source vertex $s$ and a target vertex $t$.

**Goal**: Find the shortest path distance from $s$ to $t$.

**Constraint**: Your algorithm must run in $O(V+E)$ time (Linear Time). (Note: Standard Dijkstra's Algorithm runs in $O(E \log V)$, which is too slow for this constraint.)

**The Solution: 0-1 BFS (Using a Deque)**

To achieve linear time, we cannot use a Priority Queue (which adds a logarithmic factor). Instead, we modify Breadth-First Search (BFS) using a Double-Ended Queue (Deque).

**Key Insight**:
- Edges with weight 0 represent "free" moves. We want to process these immediately to see how far we can get without increasing the cost.
- Edges with weight 1 represent "standard" moves. We process these later, maintaining the BFS level structure.

**The Algorithm**:

1. **Initialization**:
   - Create a distance array `dist[]` initialized to $\infty$. Set `dist[s] = 0`.
   - Create a Deque (Double-Ended Queue) and push `s` to the front.

2. **Process Loop**:
   - While the Deque is not empty:
     - Pop vertex `u` from the **FRONT**.
     - For each neighbor `v` connected by edge weight `w`:
       - **Relaxation**: If `dist[u] + w < dist[v]`:
         - Update `dist[v] = dist[u] + w`.
         - **The Critical Decision**:
           - If `w == 0`: Push `v` to the **FRONT** of the Deque.
           - If `w == 1`: Push `v` to the **BACK** of the Deque.

3. **Output**:
   - Return `dist[t]`.

**Why it Works (Correctness)**

The Deque maintains a strict monotonic property: at any point in time, the distances of nodes in the Deque will differ by at most 1.
- The nodes at the front have distance $k$.
- The nodes at the back have distance $k+1$.

When we process a node with distance $k$:
- A 0-weight edge keeps the distance at $k$. We push to the Front to extend the current "wave" of exploration.
- A 1-weight edge increases distance to $k+1$. We push to the Back to process it in the next "wave."

This effectively simulates Dijkstra's logic but uses the simple properties of 0 and 1 to skip the sorting step.

**Complexity Analysis**

- **Time**: Every vertex is pushed and popped from the Deque at most once. Each edge is relaxed once. Operations on a Deque (push front, push back, pop) are $O(1)$.
  - **Total**: $O(V+E)$.
- **Space**: $O(V)$ to store distances and the Deque.

### Correct Algorithm for Negative Weights
Use **Bellman-Ford Algorithm** instead:
- Time Complexity: O(VE)
- Can detect negative cycles
- Relaxes all edges V-1 times

```cpp
// Bellman-Ford (handles negative weights and detects negative cycles)
// edges: list of directed edges (u, v, w)
bool bellmanFord(const vector<tuple<int,int,int>>& edges, int V, int src, vector<long long>& dist) {
    dist.assign(V, LLONG_MAX);
    dist[src] = 0;

    // Relax all edges V-1 times
    for (int i = 0; i < V - 1; i++) {
        bool changed = false;
        for (auto [u, v, w] : edges) {
            if (dist[u] == LLONG_MAX) continue;
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                changed = true;
            }
        }
        if (!changed) break; // Early exit if no updates
    }

    // Check for negative cycles: if we can relax once more, cycle exists
    for (auto [u, v, w] : edges) {
        if (dist[u] != LLONG_MAX && dist[u] + w < dist[v]) {
            return false; // Negative cycle reachable from src
        }
    }
    return true;
}
```

Why V-1 iterations?
- Any simple shortest path has at most V-1 edges. After i-th pass, all shortest paths using at most i edges are settled, so V-1 passes suffice to propagate all improvements without cycles.

How to detect negative cycles?
- Run one additional pass. If any edge (u, v, w) can still be relaxed (i.e., dist[u] + w < dist[v]), there exists a reachable negative-weight cycle.

Complexity:
- Time: O(VE)
- Space: O(V)

Note on Undirected Graphs with Bellman-Ford
- Bellman-Ford is defined on directed graphs. For an undirected weighted graph, convert each undirected edge (u, v, w) into two directed edges: (u → v, w) and (v → u, w).
- Warning: If any undirected edge has a negative weight (w < 0), those two directed edges form a 2-edge negative cycle of total weight 2w < 0. In that case, shortest paths are undefined; Bellman-Ford will correctly flag a negative cycle.

Example conversion (undirected to directed)
```cpp
// Undirected adjacency list: adj[u] = vector of {v, w}
vector<vector<pair<int,int>>> adj(V);
// ... fill adj with undirected edges ...

vector<tuple<int,int,int>> edges; // directed edge list for Bellman-Ford
for (int u = 0; u < V; ++u) {
    for (auto [v, w] : adj[u]) {
        if (u < v) { // ensure each undirected edge produces exactly two directed edges
            edges.emplace_back(u, v, w); // u -> v
            edges.emplace_back(v, u, w); // v -> u
        }
    }
}
```

Negative-edge caveat example
```
Undirected: 0 --(-5)-- 1
Converted:  0 → 1 (-5) and 1 → 0 (-5)
Cycle weight = -5 + -5 = -10 < 0 ⇒ negative cycle detected
```

### Problem: Finding a Negative Cycle

**Problem Statement**: Given a weighted, directed graph $G=(V,E)$ and a source vertex $s$. The graph contains at least one negative-weight cycle reachable from $s$. **Goal**: Design an efficient algorithm to list the vertices of one such negative-weight cycle.

**Input**: Graph $G$, Source $s$.
**Output**: A list of vertices $[v_1, v_2, \dots, v_k, v_1]$ forming a cycle where $\sum w(e) < 0$.
**Constraint**: Time complexity should be $O(V \cdot E)$.

**The Solution Algorithm**

We utilize the Bellman-Ford Algorithm as a "Black Box" subroutine, but we modify how we handle the detection step.

1. **Step 1: Bellman-Ford Relaxation (The Setup)**
   - Initialize `dist[s] = 0` and all other distances to $\infty$.
   - Initialize `parent` array to track the path.
   - Run the standard relaxation loop $V$ times.
   - **Detection**: In the $V$-th iteration, if you encounter an edge $(u,v)$ that can still be relaxed (i.e., `dist[u] + w < dist[v]`), stop immediately.
   - Let $v$ be the node that was just updated. Let's call it `detected_node`.

2. **Step 2: The Safety Rewind (The Fix)**
   - **Problem**: `detected_node` might be on a "tail" downstream from the cycle, not in the cycle itself.
   - **Action**: Trace the parent pointers backwards exactly $V$ times.
     - `curr = detected_node`
     - Loop $V$ times: `curr = parent[curr]`
   - **Result**: The node `curr` is now guaranteed to be a vertex strictly inside the negative cycle.

3. **Step 3: Cycle Extraction**
   - Now that we are inside the loop, trace parent pointers one last time to record the cycle.
   - Initialize list `cycle`.
   - `start_node = curr`.
   - **Do-While Loop**:
     - Add `curr` to `cycle`.
     - `curr = parent[curr]`
     - Stop when `curr == start_node`.
   - The list `cycle` (reversed) is your answer.

**Proof of Correctness: Why Rewind V Times?**

- **The Graph Structure**: The subgraph formed by the parent pointers consists of paths that eventually merge into cycles. This creates a "Rho" ($\rho$) or "Lasso" shape: a linear path (Tail) leading into a loop (Cycle).
- **The Pigeonhole Principle Proof**:
  - **The House**: The graph has exactly $V$ unique vertices (rooms).
  - **The Walk**: When we trace parent pointers back $V$ steps, we visit a sequence of $V+1$ vertices (the starting node + $V$ predecessors).
  - **The Guarantee**: By the Pigeonhole Principle, since we visited $V+1$ positions but there are only $V$ available vertices, we MUST have visited at least one vertex twice.
  - **The Conclusion**: Visiting a vertex twice means we have closed a loop.
  - It is impossible to walk $V$ steps in a straight line (a Tail) because a simple path can have at most $V-1$ edges.
  - Therefore, by step $V$, we have definitively exhausted the tail and are trapped inside the cycle.

**Practice Problems**:

- [Dijkstra Algorithm](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix)
- [Shortest Path In Weighted Undirected Graph](https://www.geeksforgeeks.org/problems/shortest-path-in-weighted-undirected-graph/1)
- [Shortest Distance In a Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/)
- [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
- [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)
 - [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)
- [Minimum Multiplications To Reach End](https://www.geeksforgeeks.org/problems/minimum-multiplications-to-reach-end/1)
- [Number Of Ways To Arrive At Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/submissions/1774018133/)
- [Distance From The Source Bellman Ford Algorithm](https://www.geeksforgeeks.org/problems/distance-from-the-source-bellman-ford-algorithm/1)
- [Minimum Cost Path With Edge Reversals](https://leetcode.com/problems/minimum-cost-path-with-edge-reversals/)
- [Minimum Cost To Convert String I](https://leetcode.com/problems/minimum-cost-to-convert-string-i/)

## Minimum Spanning Tree (MST)

**Definition**: For a connected, undirected, weighted graph, a Minimum Spanning Tree is a subset of edges that connects all vertices, has no cycles, and has the minimum possible total weight. An MST always has exactly `V - 1` edges.

### Small Example
```
Vertices: 0, 1, 2, 3
Edges (undirected):
0--1 (1), 0--2 (4), 1--2 (2), 1--3 (5), 2--3 (3)

One MST: {0--1(1), 1--2(2), 2--3(3)} → total weight = 6
```

### Prim's Algorithm (Min-Heap)
Key idea: Start from any vertex and grow the tree by repeatedly adding the lightest edge that connects the current tree to a new vertex. We can store `(weight, node, parent)` directly in the heap and avoid a separate parent array.

```cpp
// Prim's MST using adjacency list and a min-heap (priority queue)
// Graph is undirected. adj[u] stores pairs (v, w) for edge u--v with weight w.
#include <bits/stdc++.h>
using namespace std;

struct Edge { int u, v; long long w; };

vector<Edge> primMST(const vector<vector<pair<int,long long>>>& adj) {
    int V = (int)adj.size();
    vector<bool> inMST(V, false);

    // Min-heap of (weight, node, parent)
    using T = tuple<long long,int,int>;
    priority_queue<T, vector<T>, greater<T>> pq;

    int src = 0;
    pq.emplace(0LL, src, -1);

    vector<Edge> mst;
    while (!pq.empty()) {
        auto [w, u, p] = pq.top(); pq.pop();
        if (inMST[u]) continue;
        inMST[u] = true;
        if (p != -1) mst.push_back({p, u, w});

        for (auto [v, wt] : adj[u]) {
            if (!inMST[v]) pq.emplace(wt, v, u);
        }
    }
    return mst;
}

int main() {
    int V = 4;
    vector<vector<pair<int,long long>>> adj(V);
    auto add = [&](int u, int v, long long w){ adj[u].push_back({v,w}); adj[v].push_back({u,w}); };
    add(0,1,1); add(0,2,4); add(1,2,2); add(1,3,5); add(2,3,3);

    auto mst = primMST(adj);
    long long total = 0;
    for (auto &e : mst) total += e.w;
    cout << "MST total weight = " << total << "\n"; // Expect 6
}
```

Complexity:
- Time: O((V + E) log V) with a binary heap
- Space: O(V + E)

### Kruskal's Algorithm (Sort + DSU)
Key idea: Sort all edges by weight and add them in increasing order, skipping any edge that would form a cycle. Use DSU to test connectivity efficiently.

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Edge { int u, v; long long w; };

// Assumes the DSU class from below is available (findParent/unionBySize)
vector<Edge> kruskalMST(int V, vector<Edge> edges) {
    sort(edges.begin(), edges.end(), [](const Edge& a, const Edge& b){ return a.w < b.w; });
    DSU dsu(V);
    vector<Edge> mst;
    for (const auto& e : edges) {
        if (dsu.findParent(e.u) != dsu.findParent(e.v)) {
            dsu.unionBySize(e.u, e.v);
            mst.push_back(e);
            if ((int)mst.size() == V - 1) break;
        }
    }
    return mst; // for connected graphs, size will be V-1
}

int main() {
    int V = 4;
    vector<Edge> edges = {
        {0,1,1}, {0,2,4}, {1,2,2}, {1,3,5}, {2,3,3}
    };
    auto mst = kruskalMST(V, edges);
    long long total = 0;
    for (auto &e : mst) total += e.w;
    cout << "MST total weight = " << total << "\n"; // Expect 6
}
```

Complexity:
- Sorting edges: O(E log E)
- DSU operations: ~O(E α(V))
- Total: O(E log E)

### Problem: Decreasing the Weight of a Non-Tree Edge

**Scenario**: You have an MST $T$. An edge $(u,v)$ that was not in $T$ gets a smaller weight. **Goal**: Find the new MST in $O(V+E)$ time.

**Algorithm (Cycle Property)**:

1. **Identify the Cycle**: Since $(u,v)$ is not in $T$, adding it to $T$ creates exactly one cycle. Use DFS or BFS on $T$ to find the unique path between $u$ and $v$.
2. **Find Max Edge**: Traverse this path and identify the edge with the maximum weight. Let's call it $e_{max}$.
3. **Compare & Swap**:
   - If $w_{new}(u,v) < w(e_{max})$, then $(u,v)$ offers a cheaper way to connect the components than $e_{max}$.
   - **Action**: Remove $e_{max}$ from $T$ and add $(u,v)$.
   - If $w_{new}(u,v) \ge w(e_{max})$, do nothing. The old $T$ is still the MST.

### Problem: Increasing the Weight of a Tree Edge

**Scenario**: You have an MST $T$. An edge $(x,y)$ that is in $T$ gets a larger weight. **Goal**: Find the new MST in $O(V+E)$ time.

**Algorithm (Cut Property)**:

1. **Remove the Edge**: Temporarily delete $(x,y)$ from $T$. This splits the tree into two disconnected components, $C_x$ and $C_y$.
2. **Identify Components**: Run a DFS or BFS starting from $x$ (using only tree edges) to mark all nodes in $C_x$. All other nodes are in $C_y$.
3. **Find Min Crossing Edge**: Iterate through all edges in the original graph $G$.
   - Look for edges $(u,v)$ where $u \in C_x$ and $v \in C_y$.
   - Find the one with the minimum weight (this could be the original $(x,y)$ with its new weight, or a different edge).
4. **Reconnect**: Add this minimum crossing edge to the tree to reconnect $C_x$ and $C_y$.

### Problem: Adding a New Vertex z

**Scenario**: You have an MST $T$. A new vertex $z$ is added with $k$ edges connecting it to the existing graph. **Goal**: Update the MST in $O(V)$ time (assuming $k \le V$).

**Algorithm (Anchor & Cycle Optimization)**:

1. **Anchor**: Find the minimum weight edge among the $k$ new edges connecting $z$ to the tree. Let's say it connects to $u_{min}$. Add $(z,u_{min})$ to the MST.
2. **Root the Tree**: Treat $u_{min}$ as the root of the tree. Run a DFS to establish parent pointers and calculate $Max[v]$ for every node $v$.
   - $Max[v]$ stores the weight of the heaviest edge on the unique path from $v$ up to the root $u_{min}$.
   - **Formula**: $Max[v] = \max(\text{weight}(v, \text{parent}), Max[\text{parent}])$.
3. **Competition**: Iterate through the remaining $k-1$ new edges. For a new edge $(z,v)$ with weight $w$:
   - Check if $w < Max[v]$.
   - If yes, it means the new edge $(z,v)$ is cheaper than the bottleneck on the old path. Swap them: Remove the edge corresponding to $Max[v]$ and add $(z,v)$.
   - **Note**: If multiple new edges try to replace the same old edge, pick the one that offers the greatest savings.

### Problem: MST for Extremely Sparse Graph (E <= V+3)

**Scenario**: The graph is connected and has very few edges (at most $n+3$ edges). **Goal**: Find the MST in $O(n)$ time.

**Algorithm (Iterative Cycle Breaking)**:

1. **Cycle Detection**: Run a DFS on the graph. Since $E \ge V$ (it's connected and has cycles), you will inevitably encounter a "back edge" (an edge to an already visited ancestor).
2. **Identify Heaviest Edge**: Trace the path from the current node back to the ancestor to identify the cycle. Find the edge with the maximum weight in this cycle.
3. **Remove**: Delete this maximum weight edge. This breaks one cycle but keeps the graph connected.
4. **Repeat**: Continue the DFS or restart it. Since there are at most $V+3$ edges and a tree has $V-1$ edges, you have at most 4 "extra" edges. You will perform this cycle-breaking step at most 4 times.

**Result**: When no cycles remain (edge count is $V-1$), the remaining edges form the MST.

### Bottleneck MST: The Decision Problem

**Problem Statement**: Give an $O(E)$-time algorithm that, given an input graph $G$ and real value $r$, determines whether the bottleneck value of $G$ is at most $r$. (i.e., Can we form a spanning tree using only edges with weight $\le r$?)

**Solution Algorithm**: The problem is equivalent to determining if the graph remains connected after removing all edges with weight strictly greater than $r$.

1. **Implicit Graph Filter**: Consider a subgraph $G' = (V, E')$ where $E'$ contains only edges $(u,v)$ such that $w(u,v) \le r$. You do not need to construct this graph explicitly; you can filter edges on the fly.
2. **Traversal (BFS or DFS)**:
   - Initialize a boolean `visited` array of size $V$ to `false`.
   - Start a BFS or DFS from an arbitrary node (e.g., node 0). Mark it as visited.
   - **Crucial Step**: During traversal, when examining neighbors of a node $u$, strictly ignore any edge $(u,v)$ if its weight is $> r$. Only add $v$ to the queue/stack if the edge weight is $\le r$.
   - Count the total number of unique nodes visited.
3. **Connectivity Check**:
   - If `count_visited == V`, the graph is connected using valid edges. Return `True`.
   - Otherwise, return `False`.

**Time Complexity**:
- The traversal visits each vertex at most once and checks each edge at most once (or twice for undirected).
- Since we visit at most $V$ nodes and $E$ edges, the complexity is $O(V+E)$.
- Since the input graph $G$ is connected, $E \ge V-1$, so this simplifies to $O(E)$.

### Bottleneck MST: The Optimization Problem

**Problem Statement**: Suppose that each edge has a distinct integer weight in the range $1, 2, \dots, |E|$. Use your algorithm from Part 1 as a black-box subroutine to find the minimum bottleneck value of $G$ in $O(E \log E)$ time.

**Solution Algorithm**: We use Binary Search on the possible answer space (the edge weights).

1. **Initialize Search Range**:
   - Set `low = 1` (Smallest possible weight).
   - Set `high = |E|` (Largest possible weight).
   - Set `ans = |E|` (To store the best feasible bottleneck found so far).

2. **Binary Search Loop**:
   - While `low <= high`:
     - Calculate `mid = (low + high) / 2`.
     - **Call Subroutine**: Run the algorithm from Part 1 with `r = mid`.
     - `check = isBottleneckAtMostR(G, mid)`
     - **Evaluate**:
       - If `check` is `True`: It means we can connect the graph using edges $\le mid$. This is a valid bottleneck. Record it (`ans = mid`) and try to find a smaller one by moving left: `high = mid - 1`.
       - If `check` is `False`: It means the graph is disconnected if we limit weights to `mid`. We need heavier edges. Move right: `low = mid + 1`.

3. **Output**:
   - Return `ans`. This is the minimum weight $r$ such that the graph is connected.

**Time Complexity**:
- The binary search range is size $E$, so the loop runs $O(\log E)$ times.
- Inside each iteration, we run the Part 1 algorithm which takes $O(E)$.
- **Total Complexity**: $O(\log E) \times O(E) = O(E \log E)$.



### Problem: The Second Best Minimum Spanning Tree

**Problem Statement**: Given a connected, undirected graph $G=(V,E)$ with distinct edge weights, let $T$ be the unique Minimum Spanning Tree (MST) of $G$. A "spanning tree" is a subset of edges that connects all vertices with no cycles. Find the Second Best Minimum Spanning Tree.

**Definition**: Among all spanning trees of $G$ that are not equal to $T$, find the one with the minimum total edge weight.

**Solution Algorithm**: This problem relies on the Cycle Property. The Second Best MST differs from the optimal MST by exactly one edge swap.

1. **Compute the Best MST**: Run Kruskal’s or Prim’s algorithm to find the optimal MST, let's call it $T$. Calculate its total weight, $W_{MST}$.
2. **Identify Candidates**: Identify all edges in the graph that are not in $T$. Let this set be $E_{non} = E \setminus T$.
3. **Calculate Swap Penalties**: For every edge $e_{new} = (u,v)$ in $E_{non}$:
   - Hypothetically add $e_{new}$ to $T$. This creates exactly one cycle.
   - Traverse this cycle (using DFS/BFS on $T$) to find the edge $e_{max}$ with the maximum weight on the path between $u$ and $v$.
   - **Calculate the "cost penalty"** of swapping $e_{max}$ for $e_{new}$:
     - $\Delta = \text{weight}(e_{new}) - \text{weight}(e_{max})$
4. **Select the Best Swap**:
   - Find the edge $e_{new}$ that yields the minimum positive $\Delta$.
   - The total weight of the Second Best MST is $W_{MST} + \Delta_{min}$.
   - The edges of the Second Best MST are $(T \setminus \{e_{max}\}) \cup \{e_{new}\}$.

**Time Complexity**:
- **Naive Approach**: $O(E \cdot V)$.
  - There are $O(E)$ non-tree edges.
  - For each edge, the DFS to find the cycle's max edge takes $O(V)$.
  - For dense graphs ($E \approx V^2$), this is $O(V^3)$.
- **Optimized Approach**: $O(E \log V)$ if using Lowest Common Ancestor (LCA) techniques to query the max edge on the path in $O(\log V)$.

### Problem: The Most Vital Edge

**Problem Statement**: Given a connected, undirected, weighted graph $G=(V,E)$ and its Minimum Spanning Tree $T$. The Vitality of an edge $e \in T$ is defined as the increase in the weight of the MST if edge $e$ is prohibited from use.

$$Vitality(e) = W(MST_{G \setminus \{e\}}) - W(T)$$

If removing $e$ disconnects the graph, $Vitality(e) = \infty$. Find the edge $e^* \in T$ that has the maximum vitality.

**Solution Algorithm (Simulation Method)**:

1. **Iterate MST Edges**: Loop through every edge $e_{target} = (u,v)$ that belongs to the MST $T$.
2. **Simulate Removal**:
   - Temporarily "delete" $e_{target}$ from the graph (or mark it as invalid).
   - This splits the tree $T$ into two disconnected components, let's call them set $S_u$ and set $S_v$.
   - **Implementation Hint**: You can identify these sets quickly using BFS/DFS starting from $u$ and $v$ without crossing the deleted edge.
3. **Find Minimum Replacement**:
   - Initialize `min_replacement_weight =` $\infty$.
   - Iterate through all non-tree edges $(x,y)$ in the original graph.
   - **Check if the edge crosses the cut**: Is $x \in S_u$ and $y \in S_v$? (or vice versa).
   - If it crosses, update: `min_replacement_weight = min(min_replacement_weight, weight(x, y))`.
4. **Calculate Vitality**:
   - If `min_replacement_weight` is still $\infty$, then $e_{target}$ is a Bridge. Return $e_{target}$ immediately as the Most Vital Edge.
   - Otherwise, calculate `current_vitality = min_replacement_weight - weight(e_target)`.
5. **Track Maximum**:
   - Compare `current_vitality` with the global maximum found so far. Update if higher.
   - Restore $e_{target}$ and proceed to the next edge.

**Time Complexity**:
- **Loop**: Runs $V-1$ times (once for each MST edge).
- **Inside Loop**:
  - Component identification (DFS/BFS): $O(V)$.
  - Scanning non-tree edges: $O(E)$.
- **Total**: $O(V \cdot (V+E)) \approx O(V \cdot E)$.
- For dense graphs, this is $O(V^3)$. This is the standard acceptable complexity for this problem.

## Disjoint Set Union (Union-Find)

Maintains a partition of elements into disjoint sets with near O(1) amortized operations.
- `find(x)`: representative of x's set (with path compression)
- `union(a, b)`: merge sets using union-by-rank or union-by-size

```cpp
#include <bits/stdc++.h>
using namespace std;

class DSU {
    vector<int> parent;
    vector<int> rank; // used by union-by-rank
    vector<int> size; // used by union-by-size
public:
    explicit DSU(int n) : parent(n), rank(n, 0), size(n, 1) {
        iota(parent.begin(), parent.end(), 0);
    }
    int findParent(int x) {
        if (parent[x] == x) return x;
        return parent[x] = findParent(parent[x]); // path compression
    }
    // Union by Rank (height heuristic)
    bool unionByRank(int u, int v) {
        int upu = findParent(u);
        int upv = findParent(v);
        if (upu == upv) return false;
        if (rank[upu] == rank[upv]) {
            parent[upu] = parent[upv];
            rank[upv]++;
        } else if (rank[upu] < rank[upv]) {
            parent[upu] = parent[upv];
        } else {
            parent[upv] = parent[upu];
        }
        return true;
    }
    // Union by Size (attach smaller to larger)
    bool unionBySize(int u, int v) {
        int upu = findParent(u);
        int upv = findParent(v);
        if (upu == upv) return false;
        if (size[upu] < size[upv]) {
            size[upv] += size[upu];
            parent[upu] = parent[upv];
        } else {
            size[upu] += size[upv];
            parent[upv] = parent[upu];
        }
        return true;
    }
    int sizeOfSet(int x) { return size[findParent(x)]; }
};

int main() {
    DSU d1(5);
    d1.unionByRank(0,1);
    d1.unionByRank(1,2);
    cout << d1.findParent(2) << "\n"; // representative of {0,1,2}

    DSU d2(5);
    d2.unionBySize(3,4);
    d2.unionBySize(2,3);
    cout << d2.sizeOfSet(4) << "\n"; // 3
}
```

Notes:
- Path compression + union heuristic → almost O(1) per operation.
- Use either rank or size consistently in one run.

### Practice Problems
- [Min Cost To Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/submissions/1783676159/?envType=problem-list-v2&envId=minimum-spanning-tree)
- [Number Of Provinces](https://leetcode.com/problems/number-of-provinces/)
- [Number Of Operations To Make Network Connected](https://leetcode.com/problems/number-of-operations-to-make-network-connected/)
- [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)
- [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
- [Count The Number Of Complete Components](https://leetcode.com/problems/count-the-number-of-complete-components)
- [Path With Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)
- [Accounts Merge](https://leetcode.com/problems/accounts-merge/description/)
- [Last Day Where You Can Still Cross](https://leetcode.com/problems/last-day-where-you-can-still-cross/)
- [Smallest String With Swaps](https://leetcode.com/problems/smallest-string-with-swaps/description/)
- [Minimize Hamming Distance After Swap Operations](https://leetcode.com/problems/minimize-hamming-distance-after-swap-operations/)
- [Detect Cycles in 2D Grid](https://leetcode.com/problems/detect-cycles-in-2d-grid/)
- [Minimum Jumps to Reach End via Prime Teleportation](https://leetcode.com/problems/minimum-jumps-to-reach-end-via-prime-teleportation)
- [Jump Game IV](https://leetcode.com/problems/jump-game-iv/description/)
- [Number Of Ways To Assign Edge Weights I](https://leetcode.com/problems/number-of-ways-to-assign-edge-weights-i/description/?envType=daily-question&envId=2026-06-11)