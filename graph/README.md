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
- [Magic Squares In Grid](https://leetcode.com/problems/magic-squares-in-grid/)

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

## Strongly Connected Components (Kosaraju's Algorithm)

Definition: In a directed graph, a strongly connected component (SCC) is a maximal set of vertices where every vertex is reachable from every other vertex.

Key idea (two DFS passes):
- Pass 1 on original graph: run DFS and push vertices to `order` by finishing time (post-order).
- Compute transpose graph G^T (reverse all edges).
- Pass 2 on G^T: process vertices in reverse `order`; each DFS tree yields one SCC.

Why it works: Any edge from an SCC to another goes forward in finish-time order; reversing edges forces DFS to stay inside one SCC when started from the latest-finishing vertex.

Complexity: O(V + E) time, O(V + E) space.

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

Practice Problem:
@https://www.geeksforgeeks.org/problems/strongly-connected-components-kosarajus-algo/1

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

**Key Issue**: Dijkstra can get stuck in an infinite loop of decreasing distances when negative weights are present.

### Simple Example with Negative Weights
```
Graph (undirected with negative weight):
A ----(-1)---- B

Or directed both ways:
A <--(-1)-- B
  --(-1)-->

Source: A
```

**Dijkstra's Execution**:
```
Initial: dist[A] = 0, dist[B] = ∞

Step 1: Process A (min distance = 0)
- Update B: dist[B] = 0 + (-1) = -1

Step 2: Process B (min distance = -1)  
- Update A: dist[A] = min(0, -1 + (-1)) = min(0, -2) = -2
- Now A has distance -2, which is less than 0!

Step 3: Process A again (min distance = -2)
- Update B: dist[B] = min(-1, -2 + (-1)) = min(-1, -3) = -3

Step 4: Process B again (min distance = -3)
- Update A: dist[A] = min(-2, -3 + (-1)) = min(-2, -4) = -4

This continues infinitely: -2, -3, -4, -5, -6, ...
```

**The Problem**: 
- Each time we process a vertex, we can make the other vertex's distance even more negative
- The distances keep decreasing indefinitely: 0 → -1 → -2 → -3 → -4 → ...
- Dijkstra never terminates because there's always a "better" (more negative) distance

### Why This Happens
1. **Dijkstra assumes shortest paths exist**: With negative cycles, there's no "shortest" path
2. **Priority queue keeps finding "better" distances**: Negative weights create infinitely decreasing distances  
3. **Algorithm never converges**: The termination condition (all vertices processed) is never met

### Why This is Problematic
**Negative cycles make "shortest path" undefined**:
- In our example, path A→B has cost -1
- Path A→B→A→B has cost -2  
- Path A→B→A→B→A→B has cost -3
- You can always find a "shorter" path by going around the cycle more times

**Dijkstra's assumption broken**:
- Dijkstra assumes that once you find the shortest distance to a vertex, it won't change
- With negative cycles, distances can always be improved by taking more loops
- The algorithm never reaches a stable state where all distances are final

### Why This Happens
1. **Greedy Choice**: Always picks unprocessed vertex with minimum distance
2. **No Backtracking**: Once processed, a vertex is never reconsidered
3. **Negative Edges**: Can create better paths through "processed" vertices

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