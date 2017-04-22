## Chapter 9: Graph Algorithms
* A graph is a pair (V, E), where V is a set of vertices and E is a collection of pairs of vertices called edges
* Directed edge ==> ordered pair of vertices
  * (u, v) - origin vertex u, destination vertex v
* Undirected edge ==> unordered pair of vertices
* Directed graph is a graph in which all edges are directed
* Undirected graph is a graph in which all edges are undirected
* **Adjacent** vertices are connected by an edge, which **is incident on** both vertices
* Graph with no cycles is a tree (acyclic connected graph)
* Two edges are parallel if they connect the same pair of vertices
* **Degree** of a vertex is the number of edges incident on it
* Subgraph is a subset of a graph’s edges that form a graph
* **Path* in a graph is a sequence of adjacent vertices
* Simple path is a path with no repeated vertices
* Cycle is a path in which the first and last vertices are the same
* A vertex is connected to another if there is a path that contains both of them
* A graph is connected if there is a path from every vertex to every other vertex
* **Directed Acyclic Graph** (DAG) is a directed graph with no cycles
* **Weighted graphs** have weights assigned to each edge
* Complete graph has all edges present
* Sparse graph has relatively few edges (fewer than V log V)
* |V| is the number of vertices
* |E| is the number of edges (ranges from 0 to V * ( V + 1 ) / 2 )

### Graph Representation
* Store vertices as an array of vertices
* Adjacency Matrix
  * Matrix of boolean values or weights to show whether two vertices are connected by an edge
  * Adj[u,v] = weight ==> u and v are connected by an edge
  * In a directed graph, Adj[u,v] = weight ==> there is an edge from u to v
  * An undirected graph only needs half of the matrix and all self edges are set to Weight
  * O(V^2) space and time to initialize
* Adjacency List
  * **Array Adj of vertices, where each element is a pointer to a linked list that contains the neighbors of that element**
  * For OOP, you can use v.neighbors = Adj[v]
  * Linked list for each node that lists the different nodes that can be visited from the current node
  * V total linked lists
  * Order of adding edges is important because it will affect the order in which all processes process edges
  * Can be inefficient for deletes because the vertex must be deleted from the vertices list AND from all adjacency lists
  * O(E+V) space
* Implicit representation
  * Use Adj(u) as a function to get the adjacency list for vertex u
  * Use v.neighbors() as a method to get the adjacency list for vertex v
  * No need to get or generate entire graph
  * Just keep getting neighbors until you find desired state
  * Good for representing graphs with many many states

### 9.5 Graph Traversals/Searches
* Start from source (similar to root of Tree)
* Depth First Search (DFS)
* Breadth First Search (BFS)

### Depth First Search (DFS)
* Similar to preorder tree traversal
* Edge types
  * Tree edge = visit new vertex
  * Forward edge = from ancestor to descendent in the forest of trees along DFS visit path (does not exist in undirected graphs)
  * Backward edge = from descendent to ancestor in the forest of trees along DFS visit path
  * Cross edge = between a tree or subtrees that are not ancestor related (does not exist in undirected graphs)
* For most problems, boolean classification (unvisited/visited) is sufficient, but some require three colors
* Use a stack to keep track of previously visited indexes
* General DFS concept
  * Start at vertex u in the graph
  * Mark vertices as visited when visited (as part of their data structure)
  * Consider the edges from u to all other vertices
  * If the edge leads to an already visited vertex, then backtrack to u
  * If it leads to an unvisited vertex, go to that vertex and that becomes current vertex (previous current is pushed to stack)
  * Repeat until reaching a dead end at u
  * Backtrack if u current vertex is unable to make progress (pop from stack)
  * Process terminates when backtracking leads back to the start vertex
* DFS traversal forms a tree (no back edges) = called DFS tree
* O(V+E) time complexity with adjacency lists
* O(V^2) for adjacency matrix

## DFS Part 2 (OCW lecture notes)
* DFS Forest consists of DFS trees and the tree edges in those trees
  * DFS trees consist of edges included in the DFS
  * DFS tree edges are the set of edges from parent u to vertex v, where the parent is not nil
* **Directed graph is acyclic if it has no back edges**
* Recursively explore graph, backtracking as necessary
* Be careful to not repeat vertices
* Vertices are either
  * White - undiscovered
  * Gray - discovered and may have undiscovered adjacent vertices
  * Black - finished
* Vertices have two timestamps (or ticks/counters)
  * v.d = discovery time (or tick)
  * v.f = finish time (or tick), when DFS finishes v’s adjacency list and blackens v
* Parenthesis Theorem
  * In a DFS, for any two vertices u and v, exactly one of the following conditions holds
    * [u.d, u.f] and [v.d, v.f] are entirely disjoint and neither u nor v is a descendant of the other in the depth-first forest
    * [u.d, u.f] is contained entirely within [v.d, v.f] and u is a descendant of v in a depth-first tree
    * [v.d, v.f] is contained entirely within [u.d, u.f] and v is a descendant of u in a depth-first tree
* White-path theorem
  * In a depth-first forest, vertex v is a descendent of vertex u IIF at the time u.d that DFS discovers u, there is a path from u to v consisting entirely of white vertices
```
parent = {s:None}
DFS-Visit(Adj, s):
  for v in Adj[s]:
    if v not in parent:
      parent[v] = s
      DFS-Visit(Adj, v)

# DFS visits and tracks all vertices in the graph
# V is the set of vertices in the graph
DFS(V, Adj):
  parent = {}
  for s in V:
    if s not in parent:
      parent[s] = None
      DFS-Visit(Adj, s)
```
* G has a cycle <==> DFS has a back edge
* Used for topological sort
  * Run DFS
  * Output reverse of finishing times/sequences of vertices
  * This works because there are no back edges (because graph is acyclic)

### Breadth First Search (BFS)
* **Used to find shortest paths**
* Similar to level order tree traversal
* Vertices are either
  * White - undiscovered
  * Gray - discovered and may have undiscovered adjacent vertices
  * Black - discovered and has all adjacent vertices discovered
* Construct breadth first tree starting at root s
  * Whenever a white vertex is discovered, it is added to the tree along with the edge
* Uses queue to store vertices at subsequent levels
* General BFS Concept
  * Starts at a given vertex, which is level 0
  * Mark vertices as visited when visited (as part of their data structure)
  * Enqueue all vertices 1 level away
  * Visit all vertices at level 1 (1 step away from start point)
  * Then visit all vertices at level 2
  * Repeat until all levels of the graph are completed
* O(V+E) time complexity with adjacency lists
* O(V^2) for adjacency matrix
```
# python BFS
BFS(s, Adj):
  level = {s:0}
  parent = {s:None}
  i = 1
  frontier = [s] # everything reachable in i-1 moves
  while frontier:
    next = [] # everything reachable in i moves
    for u in frontier:
      for v in Adj[u]:
        if v not in level:
          level[v] = i
          parent[v] = u
          next.append(u)
    frontier = next
    i += 1
```
* parent points back to s (source) and form shortest path back to s from any given vertex

### DFS vs BFS
* DFS is more memory efficient (does not require storage of child pointers on each level)
* Usage depends on whether it is important to get to the bottom of the tree or whether the desired data is near the top of the tree
* BFS is better for shortest paths

### 9.6 Topological Sort
* Topological sort is an ordering of vertices in a DAG in which each node comes before all nodes to which it has outgoing edges
  * An example is course prerequisites in a major
* If all pairs of consecutive vertices in the sorted order are connected by edges, then they form a directed Hamiltonian path, and the sort order is unique
* Otherwise, if the Hamiltonian path does not exist, the DAG can have 2+ topological sort orderings
* General Topological Sort Algorithm
  * Calculate indegree for all vertices (number of vertices leading into it) and store locally for each vertex
  * Enqueue all vertices of indegree 0
  * While queue is not empty
    * Dequeue vertex v as next vertex in sort order
    * Decrement all indegrees of edges adjacent to v
    * Enqueue a vertex as soon as its indegree falls to 0
* O(V+E) time complexity with adjacency lists

### 9.7 Shortest Path Algorithms
* GOAL: ∂(u, v) = { min( weighted_path from u to v if there exists any such path ), INF otherwise }
* Maintain two data structures
  * d(v) = current total path weight to v from source
  * ∏(v) = predecessor on current best path to v from source
* General structure, assuming no negative cycles
  * **Initialize for all u in V:**
    * **d[v] = INF**
    * **∏[u] = nil**
    * **d[s] = 0**
  * Repeat until all E have d[v] <= d[u] + w(u,v)
    * select edge (u,v) … somehow
    * if d[v] > d[u] + w(u,v)
      * d[v] = d[u] + w(u,v)
      * ∏[v] = u
* Utilizes the notion of relaxation = testing an edge to see if we can improve the current shortest path estimate
  * Relax an edge when it is used to update the distance to a vertex
  * Parent is then updated
  * Shortest path algorithms only differ in how many times they relax edges and the order in which they relax edges
  * Dijkstra relaxes each edge only one time
  * Bellman-Ford relaxes each edge 
* **Optimal substructure = subpaths of a shortest path are shortest paths**
* Triangle inequality
  * ∂(s, v) <= ∂(s, u) + w(u, v)

### Further Shortest Path Notes
* Unweighted graph, weighted graph, weighted graph with negative edges
* Unweighted shortest path
  * BFS
  * Newly discovered vertices have a distance of their parents distance plus one
  * Set their parent to their parents node

### Weighted DAG Shortest Path (non-negative edges)
* Topological sort the DAG
  * Path from u to v implies that u is before v in ordering
* One pass over vertices in topologically sorted order
  * Relax each edge that leaves each vertex
* O(V+E)

### Dijkstra (non-negative, weighted graph)
* Complexity
  * Theta(V^2 + E) with array for queue
  * Theta(V log V + E log V) for min heap
    * O(log V) for extract min (and update heap)
    * O(log V) for decrease key operation
  * Theta(v log V + E) for Fibonacci heap
    * O(log V) for extract min (and update heap)
    * O(1) for decrease key operation
* Generalization of breadth first search
* BFS cannot guarantee that the vertex at the front of the queue is the closest to the source vertex in terms of weighted distance
* Uses greedy algorithm: always picks the next closest vertex to the source
* Uses a priority queue to store on visited vertices by distance from source (instead of a regular queue in regular BFS)
  * **As new vertices are discovered they are added to the priority queue**
* Does not work on graphs with negative edges
* Distance for each vertex is stored as the total weighted distance from the source
* A distance can be updated if a new shorter distance is found
  * The element is updated in the priority queue with this new distance
* Disadvantages are doing a blind search which wastes resources and not being able to handle negative edges
```
Dijkstra(G, w, s):
  Initialize(G, s)
  S = Ø                       // set of vertices whose shortest path from s has been found
  Q = G.V                   // set of vertices whose shortest path from s remains yet to be found
                                  // priority queue, prioritized by v.d = total distance from s thus far
  while Q != Ø:
    u = extract-min(Q)
    S = S U {u}            // add u to S
    for v in Adj(u):
      Relax(u, v, w)

Initialize(G, s):
  for v in G:
    v.π = nil
    v.d = INF
  s.d = 0

Relax(u, v, w):
  if v.d > u.d + w(u,v):
    v.d = u.d + w(u,v)
    v.π = u
```

### Bellman-Ford
* Bellman-Ford Intuition
  * With each pass from 1 up to V-1, you're establishing ∂ for the nodes each level further away from s
  * Thus, each pass creates progressively more optimal subpaths until they cannot be optimized further
  * After V-1 passes, all ∂ will have been found
* Nodes unreachable due to negative weight cycles will have ∂(s, v) = undef
* Nodes unreachable otherwise will have ∂(s, v) = INF
* Initialize regular queue with s
* Initialize hash table to indicate which vertices are in the queue
* At each iteration, dequeue v
* Find all vertices w adjacent to v such that: dist[v] + weight(v,w) < old dist[w]
* Then update old distance and path for w and enqueue if not already there
* Repeat until queue is empty
* O(E*V) with adjacency lists
* Disadvantage is that it does not work if there are negative-cost cycles
````
Bellman-Ford(G, w, s):
  Initialize(G, s)
  for i=1 to size(V)-1:
    for each edge (u, v):
      Relax(u, v, w)

  // check for negative weight cycle
  for each edge (u, v):
    if v.d > u.d + w(u, v):
      Report negative weight cycle exists
```

### Bidirectional Search
* Source s, Target t
* Run Dijkstra, alternating forwards from s and backwards from t, when they meet the algorithm will be complete
* Maintain priority queues for Q_forward, Q_backward
* When an element has been extracted from both, the frontiers will have met and the search can be terminated
* **∂(s, t) = min(d_forward[x] + d_backward[x])**
  * x may or may not be the same vertex that terminates the search

### 9.8 Minimum Spanning Tree (MST) Algorithms
* Spanning Tree = subgraph (that is a tree) that contains all vertices in a graph AND has minimum total weight
* Prim’s Algorithm
  * Almost exact same as Dijkstra
  * Relax function is all that differs:
```
PrimRelax(u, v, w):
  if v.d > w(u, v):
    v.d = w(u, v)
```
* Kruskal’s Algorithm
