---
title: Graph Theory
date: 2024-11-29 20:14:00 +0200
categories: [University, math]
tags: [Math, Graph Theory]
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
- **Weighted Graph**: A graph where edges have associated weights, typically representing costs, distances, or capacities.  

---

### Key Concepts  
- **Incident Vertices**: Two vertices are incident if they are connected by an edge.  
- **Adjacent Vertices**: Two vertices connected by an edge.  
- **Degree of a Vertex**: The number of edges connected to the vertex.  
- **Isolated Vertex**: A vertex with no edges connected to it.  
- **Walk**: A sequence of vertices and edges where each edge's endpoints are adjacent vertices.  
- **Trail**: A walk where no edge is repeated.  
- **Path**: A trail where no vertex is repeated.  
- **Cycle**: A path that starts and ends at the same vertex.  

---

### Eulerian and Semi-Eulerian Graphs  
- **Eulerian Graph**: A graph is Eulerian if:  
  1. It is connected, and  
  2. Every vertex has an even degree.  

- **Semi-Eulerian Graph**: A graph is semi-Eulerian if:  
  1. It is connected, and  
  2. Exactly two vertices have odd degrees.  

If a graph is semi-Eulerian, the Eulerian path must start and end at vertices with odd degrees.  

---

### Algorithms for Eulerian Paths and Circuits  

**Fleury's Algorithm**  
Steps:  
1. Start at any vertex and choose an edge.  
2. Traverse edges one by one, avoiding bridges unless no other options remain.  
3. Continue until all edges are traversed.  

**Hierholzer's Algorithm**  
Steps:  
1. Start at any vertex and traverse a circuit until returning to the starting vertex, marking all edges traversed.  
2. If any vertex in the circuit has unmarked edges, start a new circuit from that vertex.  
3. Merge all circuits into a single Eulerian circuit.  

---

### Hamiltonian Cycles and Paths  
- **Hamiltonian Cycle**: A cycle that visits every vertex exactly once and returns to the starting vertex.  
- **Hamiltonian Path**: A path that visits every vertex exactly once.  

---

### Dijkstra's Algorithm  

**Purpose**: Finds the shortest path from a source vertex to all other vertices in a weighted graph with non-negative edge weights.  

#### Steps:
1. **Initialization**:
   - Assign a tentative distance of infinity ($\infty$) to all vertices, except the source vertex, which gets a distance of 0.
   - Mark all vertices as unvisited.  
   - Set the source vertex as the current vertex.  

2. **Update Distances**:
   - For the current vertex, consider all its unvisited neighbors.
   - Calculate the tentative distance to each neighbor as:
     $$
     \text{Tentative distance} = \text{Current vertex distance} + \text{Edge weight}
     $$
   - If this distance is smaller than the previously recorded distance, update it.

3. **Mark as Visited**:
   - Mark the current vertex as visited (it will not be checked again).  
   - Select the unvisited vertex with the smallest tentative distance as the new current vertex.

4. **Repeat**:
   - Repeat steps 2 and 3 until all vertices have been visited or the smallest tentative distance among the unvisited vertices is infinity (indicating unreachable vertices).

5. **Result**:
   - The shortest path from the source to each vertex is recorded in the distance table.