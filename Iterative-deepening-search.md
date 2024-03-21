# Iterative Deepening Search

Iterative Deepening Search (IDS) or Iterative Deepening Depth-First Search (IDDFS) is a state space/graph search strategy in which a depth-limited search is run repeatedly, increasing the depth limit with each iteration until the goal is found. IDS combines the space-efficiency of depth-first search with the optimality and completeness of breadth-first search. This search strategy is particularly useful in infinite or unknown search spaces where the depth of the solution is not known a priori.

## Overview

Iterative Deepening Search systematically explores the search space by gradually expanding the search horizon. It starts by exploring shallow depths of the graph or tree and incrementally explores deeper levels, hence ensuring that the shallowest goal node is found first. The algorithm is simple, yet effective, for solving puzzles, pathfinding problems, and in situations where the solution depth is unknown or the search space is unbounded.

## How It Works

Iterative Deepening Search performs a series of depth-limited searches (similar to Depth-First Search) with increasing depth limits until the goal is found. Initially, the depth limit might be set to 1, and it increases by 1 with each iteration. If a solution is not found within a given depth limit, the algorithm restarts from the initial state with an increased depth limit.

### Algorithm Steps

1. **Initialization**: Set the initial depth limit to a small number, typically 0 or 1.
2. **Depth-Limited Search**: Perform a depth-first search up to the current depth limit.
   - If the goal is found, return the solution.
   - If not, increase the depth limit and repeat the search.
3. **Repeat** the depth-limited search with an incremented depth limit until the goal is found or the search space is fully explored.

## Properties

- **Completeness**: IDS is complete, meaning it will find a solution if one exists, as long as the branching factor is finite.
- **Optimality**: IDS is optimal for unweighted graphs or trees, ensuring that the shallowest goal node is found first.
- **Time Complexity**: Similar to breadth-first search, the time complexity is O(b^d), where b is the branching factor and d is the depth of the shallowest solution.
- **Space Complexity**: The space complexity is O(bd), which is significantly lower than that of breadth-first search, making IDS particularly useful for memory-constrained environments.

## Advantages

- **Memory Efficiency**: IDS uses much less memory than breadth-first search because it stores only a single path from the root to a leaf node, along with the remaining unexplored sibling nodes for each node in the path.
- **Optimality and Completeness**: Unlike depth-first search, IDS is both optimal and complete when dealing with unweighted graphs or trees.
- **Flexibility**: Suitable for search problems where the depth of the solution is unknown or the search space is infinite or very large.

## Applications

- **Puzzle Games**: Effective for games like Sudoku or puzzles where the solution depth is unknown.
- **Pathfinding**: Used in scenarios where a path exists within a bounded area or map but the exact depth is unknown.
- **Artificial Intelligence**: Planning and decision-making in AI, where solutions at minimal depths are preferred.

## Limitations

- **Overhead**: IDS revisits nodes multiple times, leading to computational overhead. However, this is offset by its memory efficiency and optimality.
- **Non-Optimality in Weighted Graphs**: IDS is optimal in unweighted graphs or trees. For weighted graphs, other algorithms like Uniform Cost Search or A* are more suitable.

## Conclusion

Iterative Deepening Search is a versatile and efficient algorithm for exploring search spaces where the depth of the solution is not known beforehand. It elegantly combines the advantages of depth-first and breadth-first searches, making it a preferred choice in constrained environments and a wide range of problem-solving scenarios.


### Demo 

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/iddfs11-1024x420.png)
![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/iddfs2-1024x420.png)
