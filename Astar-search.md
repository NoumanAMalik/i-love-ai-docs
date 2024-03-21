# A* Search Algorithm

A* (pronounced "A-star") is a search algorithm that finds the shortest path between an initial node and a target node within a weighted graph. It combines features of uniform-cost search and pure heuristic search to efficiently compute optimal paths. A* was introduced in 1968 by Peter Hart, Nils Nilsson, and Bertram Raphael. The key to its efficiency and effectiveness is its use of a heuristic function to estimate the cost of reaching the goal from any node.

## Overview

A* search is widely recognized for its performance and accuracy in pathfinding and graph traversal. The algorithm maintains a priority queue, where it places all partial paths according to their cost combined with an estimated cost to the goal from that node. This combination helps A* to prioritize paths that are both low-cost and likely to lead towards the goal.

## How A* Works

A* selects the path that minimizes:

```plaintext
f(n) = g(n) + h(n)
f(n) is the total estimated cost of path through node n.
g(n) is the cost from the start node to n.
h(n) is the heuristic estimate of the cheapest path from n to the goal.
```

The heuristic function **h(n)** is problem-specific. For the algorithm to be efficient and accurate, the heuristic needs to be admissible, meaning it never overestimates the cost to reach the goal.

## Algorithm Steps

## Initialization
Start with a priority queue (or open set). The initial node is added with f(n) calculated.

## Loop until the goal is reached or the open set is empty
1. Remove the node with the lowest f(n) value from the open set.
2. Expand this node and for each successor:
   - Calculate g(n) and h(n), and hence f(n).
   - If a node with the same position as successor is in the open set with a lower f(n), skip this successor.
   - Otherwise, add (or update) the successor to the open set.

## Construct Path
Once the goal is reached, backtrack through the nodes to construct the path from start to goal.

# Properties
- **Completeness:** A* is complete, meaning it will always find a solution if one exists.
- **Optimality:** A* is optimal, provided that the heuristic function h(n) is admissible.
- **Time Complexity:** While potentially exponential in the worst case, A*'s efficiency heavily depends on the heuristic. In the best case, the time complexity is much lower.
- **Space Complexity:** Can be high since it stores all explored and frontier nodes.

# Applications
- **Pathfinding in Maps:** Widely used in GPS and navigational systems to find the shortest route between two locations.
- **Game Development:** For character movement and AI in video games.
- **Robotics:** Path planning for robots to move in an environment.
- **Networks:** Finding the shortest path in networking routing protocols.

# Heuristic Function
The choice of heuristic affects the performance and accuracy of A*. Common heuristics include:
- Manhattan distance (for grid-based maps).
- Euclidean distance (for direct distance measurement).
- Domain-specific heuristics (customized to the problem space).

# Limitations
- **Memory Usage:** A* can be memory intensive due to the need to store all generated nodes.
- **Heuristic Accuracy:** The efficiency of A* is highly dependent on the heuristic's accuracy. Poor heuristics can lead to suboptimal performance.

# Conclusion
A* search algorithm remains one of the most popular and effective algorithms for finding the shortest path between nodes in a graph. Its success and efficiency hinge on the use of an effective heuristic function. By carefully selecting or designing heuristics, A* can be adapted to a wide range of problems beyond mere pathfinding, demonstrating its versatility and power in various applications.

### Demo 

![](https://i.imgur.com/YjfzxXY.gif)
