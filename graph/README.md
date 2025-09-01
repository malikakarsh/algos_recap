# Graph Representations: Time and Space Complexity

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