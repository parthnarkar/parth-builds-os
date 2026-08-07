# Graphs

Graphs are non-linear data structures used to represent relationships between different entities. A graph consists of a set of vertices (nodes) and edges connecting them.

## Basic Terminology

* **Vertex (Node):** Fundamental unit of a graph.
* **Edge:** Connection between two vertices.
* **Degree:** Number of edges connected to a vertex.
* **Path:** Sequence of vertices connected by edges.
* **Cycle:** A path that starts and ends at the same vertex.
* **Connected Graph:** A graph where every vertex is reachable from every other vertex.
* **Disconnected Graph:** A graph with one or more unreachable components.

## Types of Graphs

### 1. Undirected Graph

Edges do not have a direction.

Example:

```text
1 ----- 2
|       |
|       |
3 ----- 4
```

### 2. Directed Graph (DAG)

Edges have a specific direction.

Example:

```text
1 → 2 → 3
↓
4
```

### 3. Weighted Graph

Each edge contains a weight or cost.

```text
A --5--> B
A --2--> C
```

### 4. Unweighted Graph

Edges do not carry any weight.

### 5. Cyclic Graph

Contains at least one cycle.

### 6. Acyclic Graph

Contains no cycles.

## Graph Representation

### 1. Adjacency Matrix

Stores connections in a 2D matrix.

**Space Complexity:** O(V²)

### 2. Adjacency List

Stores neighbors of each vertex.

**Space Complexity:** O(V + E)

Preferred for sparse graphs.

## Graph Traversal Algorithms

### Breadth First Search (BFS)

* Uses Queue
* Explores level by level
* Useful for shortest path in unweighted graphs

### Depth First Search (DFS)

* Uses Recursion or Stack
* Explores as deep as possible before backtracking
* Useful for cycle detection and connected components

## Important Graph Algorithms

* Breadth First Search (BFS)
* Depth First Search (DFS)
* Topological Sort
* Dijkstra's Algorithm
* Bellman-Ford Algorithm
* Floyd Warshall Algorithm
* Prim's Algorithm
* Kruskal's Algorithm
* Disjoint Set Union (DSU)

## Common Applications

* Social Networks
* GPS Navigation
* Network Routing
* Dependency Resolution
* Recommendation Systems
* Web Crawlers
* Computer Networks

## Problem Sets

* Set 1: Graph Representation, BFS and DFS
* Set 2: Connected Components and Cycle Detection
* Set 3: Topological Sort and Shortest Path Algorithms
