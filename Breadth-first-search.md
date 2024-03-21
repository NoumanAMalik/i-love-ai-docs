# Breadth-First Search (BFS)

Breadth-first search (BFS) is a fundamental algorithm for traversing or searching tree or graph data structures. It starts at an arbitrary node of a graph and explores the neighbor nodes first, before moving to the next level of neighbors. BFS is particularly useful for finding the shortest path on unweighted graphs.

## Overview

BFS explores a graph in the broadest manner possible by visiting all neighbors of a node before proceeding to their children. This property makes it an ideal algorithm for finding the shortest path in terms of the number of edges from the start node to every other node in an unweighted graph. BFS can be used in various applications, such as network broadcasting, garbage collection algorithms, and solving puzzles or mazes.

## Algorithm

The BFS algorithm can be described as follows:

1. **Initialize**: Start by putting any one of the graph's vertices at the back of a queue.
2. **Process**: Take the front item of the queue and add it to the visited list.
3. **Neighbor Exploration**: Create a list of that vertex's adjacent nodes. Add the ones which aren't in the visited list to the back of the queue.
4. **Loop**: Keep repeating steps 2 and 3 until the queue is empty.

### Psudocode
    function BREADTH-FIRST-SEARCH(problem) returns a solution node or failure
        node←NODE(problem.INITIAL)
        if problem.IS-GOAL(node.STATE) then return node
            frontier←a FIFO queue, with node as an element
            reached← {problem.INITIAL}
            while not IS-EMPTY(frontier) do
                node←POP(frontier)
                for each child in EXPAND(problem, node) do
                    s←child.STATE
                    if problem.IS-GOAL(s) then return child
                    if s is not in reached then
                        add s to reached
                        add child to frontier
    return failure
    function UNIFORM-COST-SEARCH(problem) returns a solution node, or failure
    return BEST-FIRST-SEARCH(problem, PATH-COST)


## Properties of BFS
Completeness: BFS is complete, meaning it will find a solution if one exists.
Optimality: BFS is optimal for finding the shortest path in an unweighted graph.
Time Complexity: The time complexity of BFS is O(V + E) for a graph represented using an adjacency list, where V is the number of vertices and E is the number of edges.
Space Complexity: The space complexity can be significant since it stores all generated nodes in memory.
## Applications
- Shortest Path and Minimum Spanning Tree for Unweighted Graph: BFS can be used to find the shortest path between two nodes in an unweighted graph.
- Peer-to-Peer Networks: BFS is used in peer-to-peer networks for searching nodes within the network.
- Crawlers in Search Engines: BFS is used by crawlers of search engines to explore the web or social networking sites.
- GPS Navigation Systems: BFS algorithms are employed to find neighboring locations from the current location at various levels of proximity.
- Path Finding Algorithms: BFS is widely used in pathfinding algorithms, such as finding the shortest path in a maze or between two locations on a map.
- Broadcasting in Network: In networking, BFS is used for broadcasting packets of information to all nodes of a network.
## Conclusion
Breadth-first search is a powerful algorithm for searching and exploring graph structures. Its ability to find the shortest path on unweighted graphs lends it to a wide range of practical applications, from GPS navigation to network broadcasting. Understanding BFS and its properties is essential for anyone looking to solve problems involving graph traversal.


### Demo
![](https://aquarchitect.github.io/swift-algorithm-club/Breadth-First%20Search/Images/AnimatedExample.gif)
