# Greedy Best-First Search

Greedy Best-First Search is a search algorithm used in navigating graphs or trees. This algorithm selects paths to explore based on an estimate of the shortest path from the current node to the goal, without considering the path already taken. It prioritizes nodes believed to be closest to the goal, making it a "greedy" approach as it makes the locally optimal choice at each step with the hope of finding the global optimum.

## Overview

Greedy Best-First Search is a specific case of the more general best-first search algorithm. It uses a heuristic function to rank nodes by their estimated distance to the goal node, hence always expanding the node that appears closest to the goal. This approach can lead to rapid convergence on a solution but does not guarantee the shortest path.

## Algorithm

The algorithm maintains a priority queue where nodes are sorted based on the heuristic value. At each step, it dequeues the node with the lowest heuristic cost (i.e., the node estimated to be closest to the goal) and explores its neighbors. This process repeats until the goal is found or there are no more nodes to explore.

### Steps

1. **Initialize** a priority queue with the starting node.
2. **Loop**: Repeat until the priority queue is empty or the goal is reached.
   - Dequeue the node with the lowest heuristic cost.
   - If this node is the goal, return the solution.
   - Otherwise, enqueue its neighbors with their corresponding heuristic costs.
3. **Return** failure if the goal is not found.

## Properties

- **Completeness**: Greedy Best-First Search is not complete; it can get stuck in loops or fail to find a solution if one exists.

- **Optimality**: It does not guarantee an optimal solution. The first path found to the goal is returned, which may not be the shortest.

- **Time Complexity**: Depends on the number of nodes and the implementation of the priority queue. Generally, it can be highly efficient in reaching a goal but does not guarantee polynomial time.

- **Space Complexity**: Since it stores all expanded nodes, the space complexity can be significant, particularly in dense graphs or large search spaces.

## Heuristic Function

The performance of Greedy Best-First Search heavily relies on the heuristic function used. An ideal heuristic function estimates the cost to reach the goal from a node accurately. Common examples include straight-line distance in spatial problems or the number of misplaced tiles in puzzle games.

## Applications

- **Pathfinding**: Used in mapping and navigation tools to find paths between locations.
- **Game AI**: Helps in making decisions that seem most promising to win.
- **Puzzle Solving**: Efficient for puzzles with a clear goal state and an applicable heuristic.

## Limitations

- Greedy Best-First Search's primary limitation is its lack of completeness and optimality. It can easily be misled by poor heuristic functions.
- The algorithm's efficiency and effectiveness are highly dependent on the heuristic's accuracy. An inappropriate heuristic can lead to poor performance or failure to find a path.

## Conclusion

Greedy Best-First Search offers a simple and often efficient method for navigating to a goal. While it does not guarantee optimality or completeness, its speed and simplicity make it a valuable tool in many contexts, especially when coupled with a well-chosen heuristic function. Understanding its limitations is crucial for applying it effectively to solve real-world problems.

### Demo 

![](https://upload.wikimedia.org/wikipedia/commons/f/f9/Greedy-search-path.gif)
