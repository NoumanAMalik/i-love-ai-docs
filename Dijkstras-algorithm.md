# Dijkstra's Algorithm

Dijkstra's Algorithm is a fundamental algorithm for finding the shortest paths between nodes in a graph, which may represent, for example, road networks. Conceived by computer scientist Edsger W. Dijkstra in 1956, it solves the single-source shortest path problem for a graph with non-negative edge path costs, producing a shortest path tree. This classic algorithm is used in various applications, from GPS navigation to network routing protocols.

## Overview

Dijkstra's Algorithm assumes a graph with non-negative edge weights and finds the shortest path from a given start node to all other nodes in the graph. The algorithm maintains a set of nodes whose shortest distance from the start node is already known and a set of nodes whose shortest distance is not yet determined. Initially, the distance to the start node is zero and infinity for all other nodes. The algorithm repeatedly selects the node with the smallest distance, calculates the distance through it to each unvisited neighbor, and updates the neighbor's distance if smaller. Marking visited nodes and moving to the next unvisited node with the smallest distance continues until all nodes are visited.

## Algorithm Steps

1. **Initialization**: Set the initial node as current. Mark all other nodes with an infinite distance. Set the initial node's distance to zero.

2. **Set Visited**: For the current node, consider all its unvisited neighbors and calculate their tentative distances through the current node. Compare the newly calculated tentative distance to the current assigned value and assign the smaller one.

3. **Mark as Visited**: When done considering all neighbors of the current node, mark the current node as visited. A visited node will not be checked again.

4. **Select Unvisited Node**: Select the unvisited node with the smallest tentative distance, set it as the new "current node," and go back to step 2.

The process repeats until all nodes are visited.

## Pseudocode

```plaintext
function Dijkstra(Graph, source):
    dist[source] ← 0                           // Initialization

    create vertex set Q

    for each vertex v in Graph:           
        if v ≠ source
            dist[v] ← INFINITY                 // Unknown distance from source to v
            prev[v] ← UNDEFINED                // Predecessor of v

        Q.add_with_priority(v, dist[v])


    while Q is not empty:                      // The main loop
        u ← Q.extract_min()                    // Remove and return best vertex
        for each neighbor v of u:              // where v has not yet been removed from Q.
            alt ← dist[u] + length(u, v)
            if alt < dist[v]
                dist[v] ← alt
                prev[v] ← u
                Q.decrease_priority(v, alt)

    return dist[], prev[]
```

## Properties
- Completeness: The algorithm will find a shortest path if one exists.
- Optimality: Provides the optimal shortest path from the start node to all other nodes in the graph.
- Time Complexity: When using a min-priority queue and adjacency list, the complexity is O((V+E) log V) where V is the number of vertices and E is the number of edges.
- Space Complexity: Requires O(V) space for storing the distance and predecessor information.
## Applications
- GPS Navigation Systems: Calculating the shortest route from one location to another.
- Network Routing Protocols: Such as OSPF (Open Shortest Path First) and IS-IS, use Dijkstra's Algorithm to calculate the shortest path through a network.
- Pathfinding in Video Games: For finding the shortest paths for moving characters between points on a map.
- Robotics: Path planning for autonomous robots.
## Limitations
- Non-Negative Weights: Dijkstra's algorithm cannot handle graphs with negative edge weights. The Bellman-Ford algorithm is a better choice for such graphs.
- Static Graphs: It assumes a static graph. If a graph changes dynamically, the algorithm needs to be rerun.
Dijkstra's Algorithm is a cornerstone of graph theory and continues to be widely used in computing and engineering for solving the shortest path problems efficiently.
