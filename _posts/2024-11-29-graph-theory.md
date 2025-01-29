---
title: Graph Theory
date: 2024-11-29 20:14:00 +0200
categories: [University, math]
tags: [math]
math: true
---

**Undirected Graph**: An ordered pair $G = (V, E)$, where $V$ is the set of vertices and $E$ is the set of edges.

---

### Types of Graphs  
- **Empty Graph**: $G = (\emptyset, \emptyset)$, a graph with no vertices and no edges.  
- **Loop**: An edge that connects a vertex to itself.  
- **Multiple Edges**: Two or more edges connecting the same pair of vertices.  
- **Multigraph**: A graph that contains loops or multiple edges.  
- **Simple Graph**: A graph without loops or multiple edges.  
- **Trivial Graph**: A graph with only one vertex and no edges.  
- **Directed Graph (Digraph)**: A graph where edges have a direction, represented as ordered pairs of vertices.  
- **Connected Graph**: A graph in which there is a path between every pair of vertices.  
- **Complementary Graph**: A graph $G'$ where edges in $G'$ are those not present in $G$.  
- **Complete Graph**: A graph in which every pair of vertices is connected by a unique edge.  
- **Bipartite Graph**: A graph whose vertices can be divided into two disjoint sets such that every edge connects a vertex from one set to a vertex in the other.  
- **Acyclic Graph**: A graph that contains no cycles or circuits.  
- **Network**: A connected graph, often weighted.  
- **Tree**: A connected acyclic graph (network).  
- **Forest**: An acyclic graph that may not be connected, consisting of multiple trees.  
- **Leaf (Leaf Node)**: A vertex with a degree of 1.  
- **Rooted Tree**: A tree with one designated vertex identified as the root.

---

### Key Concepts  
- **Incident Vertices**: Two vertices are incident if they are connected by an edge.  
- **Adjacent Vertices**: Two vertices connected by an edge.  
- **Isolated Vertex**: A vertex with no edges connected to it.  
- **Walk**: A sequence of vertices and edges where each edge's endpoints are adjacent vertices.  
- **Trail (Circuit)**: A walk where no edge is repeated.  
- **Path (Cycle)**: A trail where no vertex (except possibly the starting and ending vertices) is repeated.

---

### Eulerian and Semi-Eulerian Graphs  
- **Eulerian Graph**: A graph is Eulerian if:  
  1. It is connected, and  
  2. Every vertex has an even degree.  
- **Semi-Eulerian Graph**: A graph is semi-Eulerian if:  
  1. It is connected, and  
  2. Exactly two vertices have odd degrees.  
   
If a graph is semi-Eulerian, the Eulerian path must start at a vertex with an odd degree.

---

### Algorithms for Minimum Spanning Trees  

#### **Kruskal's Algorithm**  
Steps:  
1. Select the edge with the smallest weight.  
2. Add it to the spanning tree if it does not form a cycle.  
3. Repeat until all vertices are connected.

#### **Prim's Algorithm**  
Steps:  
1. Choose any starting vertex.  
2. Select the smallest connected edge that does not form a cycle.  
3. Repeat until all vertices are included.

---

### Algorithms for Eulerian Paths and Circuits  

#### **Fleury's Algorithm**  
Steps:  
1. Start at any vertex and choose an incident edge.  
2. Traverse edges one by one. If traversing an edge would split the graph into two disconnected components, skip that edge (unless no other options remain).  
3. Continue until all edges are traversed.  

#### **Hierholzer's Algorithm**  
Input: A connected semi-Eulerian graph.  
Steps:  
1. Start at any vertex $v$.  
2. Traverse a circuit until returning to $v$, marking all edges traversed.  
3. If any vertex in the circuit has unmarked edges, start a new circuit from that vertex.  
4. Merge all circuits into a single Eulerian circuit.

---

### Eulerization and Semi-Eulerization  

#### **Eulerization**  
The process of adding edges to make a graph Eulerian.  
Steps:  
1. Duplicate edges to ensure all vertices have an even degree.

#### **Semi-Eulerization**  
The process of adding edges to make a graph semi-Eulerian.  
Steps:  
1. Duplicate edges so that exactly two vertices have an odd degree.

---

### Hamiltonian Cycles and Paths  

- **Hamiltonian Cycle**: A cycle that visits every vertex exactly once and returns to the starting vertex.  
- **Hamiltonian Path**: A path that visits every vertex exactly once.

---

### Methods for Finding Hamiltonian Cycles  

#### **Nearest Neighbor**  
Input: Weighted complete graph.  
Steps:  
1. Start at a vertex $v$.  
2. Select the edge with the smallest weight incident to $v$ (breaking ties randomly).  
3. Move to the adjacent vertex and repeat until all vertices are visited.  
4. Return to the starting vertex and calculate the total weight.

#### **Cheapest Link**  
Steps:  
1. Select the smallest edge in the graph.  
2. Continue adding the smallest edges, ensuring no vertex has more than two edges and no cycle is formed prematurely.  
3. Stop when the path includes all vertices.

#### **Nearest Insertion**  
Steps:  
1. Start with a single vertex.  
2. Select the vertex closest to any vertex in the existing path and insert it into the path to minimize the total weight.  
3. Repeat until all vertices are included.
