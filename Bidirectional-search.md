# Bidirectional Search

Bidirectional search is an efficient search strategy that finds the shortest path between a start node and a goal node in a graph. The search simultaneously explores the graph from both the start node and the goal node, meeting in the middle. This approach can significantly reduce the search space and time, especially in dense graphs or large problem spaces.

## Overview

The key idea behind bidirectional search is to run two simultaneous searches—one forward from the start node and the other backward from the goal node—until the two searches meet. This strategy is based on the assumption that each of the two searches has to cover less ground than a single search would, making the overall process faster and more efficient.

Bidirectional search is particularly useful in pathfinding problems, such as navigating maps or solving puzzles, where the goal state is clearly defined. It is most effective in undirected graphs where each action is reversible.

## Algorithm

The algorithm alternates between expanding nodes from the start node forward and expanding nodes from the goal node backward. The process continues until a common node is found in both the forward and backward search spaces. Once this intersection is discovered, the shortest path from the start node to the goal node can be reconstructed by joining the two halves at the intersecting node.

### Steps

1. **Initialize**: Start with two open lists: one for the forward search (starting from the initial node) and one for the backward search (starting from the goal node).
2. **Loop**: Continue until a node is found that is present in both the forward and backward open lists.
   - In each iteration, expand the node with the least total estimated cost in one direction and then switch to the other direction.
3. **Check for Intersection**: After each expansion, check if any of the newly expanded nodes are already present in the opposite open list.
4. **Construct Path**: Once an intersection is found, reconstruct the path from the start node to the goal node by combining the paths from both search directions at the intersecting node.

## Pseudocode

```plaintext
function BidirectionalSearch(start, goal):
    initialize forwardOpenList and backwardOpenList
    forwardOpenList.add(start)
    backwardOpenList.add(goal)
    
    while forwardOpenList is not empty and backwardOpenList is not empty:
        forwardNode = forwardOpenList.remove()
        if forwardNode is in backwardOpenList:
            return reconstructPath(forwardNode, backwardOpenList)
        
        for each child of forwardNode:
            forwardOpenList.add(child)
        
        backwardNode = backwardOpenList.remove()
        if backwardNode is in forwardOpenList:
            return reconstructPath(backwardNode, forwardOpenList)
        
        for each child of backwardNode:
            backwardOpenList.add(child)
    
    return failure
```

## Properties

- **Completeness**: Bidirectional search is complete if the branching factor is finite and the graph is finite. This means that if a path exists, the algorithm is guaranteed to find it.

- **Optimality**: The algorithm is optimal if the search space is a tree, the graph is undirected, and costs are uniform. It remains optimal in weighted graphs if adapted with consistent heuristics.

- **Time Complexity**: The time complexity is significantly less than that of unidirectional search methods like Breadth-First Search (BFS), particularly in large search spaces. The exact complexity depends on the structure of the search space and the methods used for searching and matching nodes between the two search fronts.

- **Space Complexity**: Bidirectional search requires storing two sets of nodes, one for the forward search and one for the backward search. This can be substantial but is generally less than the space needed for an equivalent unidirectional search.

## Applications

- **Pathfinding**: The algorithm is widely used in GPS and mapping applications to find the shortest route between two locations.

- **Puzzle Solving**: It is effective for puzzles where the goal state is known, such as Rubik's Cube or sliding tile puzzles.

- **Artificial Intelligence**: Bidirectional search is utilized in game AI for planning and decision-making where a clear goal state can be defined.

## Limitations

- **Implementation Complexity**: Implementing bidirectional search can be complex, especially in determining when and how the two searches meet.

- **Suitability**: The algorithm is not suitable for problems where the goal state is not clearly defined or when actions are not reversible.

- **Efficiency Dependence**: Its efficiency depends on the ability to efficiently check for intersections between the sets of nodes expanded by the forward and backward searches.


Bidirectional search offers a powerful approach for finding the shortest path between two nodes, significantly reducing the search time and space required compared to traditional unidirectional search algorithms. Its effectiveness in specific applications depends on the structure of the search space and the problem at hand. When implemented correctly, it can provide an efficient solution for various problems in pathfinding, puzzle solving, and AI.
