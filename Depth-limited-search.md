# Depth-Limited Search

Depth-Limited Search (DLS) is a variant of Depth-First Search (DFS), which explores a graph or tree to a specified depth limit. This strategy prevents the search from going too deep into paths that are likely irrelevant or too costly to explore fully. DLS is particularly useful in situations where the search space is vast or infinite, and a solution is expected to be found within a certain depth.

## Overview

DLS is implemented similarly to DFS but with an added constraint on the depth of the search. It explores each branch of the graph or tree up to a specified depth limit and ignores any paths that exceed this limit. This approach helps to mitigate the risk of getting caught in infinite loops or excessively deep searches in large or infinite search spaces. DLS can be used as a standalone search strategy or as part of more complex algorithms, such as Iterative Deepening Depth-First Search (IDDFS).

## Algorithm

The DLS algorithm starts at the root node (or any arbitrary node, in the case of a graph) and explores as far as possible along each branch before backtracking. However, it stops exploring further down a path once it reaches the predefined depth limit.

### Steps

1. **Start** at the root node (or any arbitrary node) and set a depth limit.
2. **Explore** each branch to the specified depth limit.
3. **Backtrack** when the depth limit is reached or a node has no unexplored neighbors.
4. **Repeat** the process for each unexplored path until a goal is found or all paths up to the depth limit have been explored.

## Pseudocode

```plaintext
function DepthLimitedSearch(node, goal, depthLimit) {
    if depthLimit == 0 and node is goal:
        return success
    else if depthLimit > 0:
        for each child in expand(node):
            result = DepthLimitedSearch(child, goal, depthLimit - 1)
            if result == success:
                return success
    return failure
}
```

## Properties
- Completeness: DLS is not complete as it may not find a solution even if one exists, due to the depth limit.
- Optimality: DLS is not optimal since it does not guarantee the shortest path to a solution.
- Time Complexity: Similar to DFS, the time complexity of DLS depends on the branching factor (b) and the depth limit (l), making it O(b^l).
- Space Complexity: Since DLS needs to store only a single path from the root to a leaf node, along with the remaining unexplored siblings for each node, the space complexity is O(b*l).
## Applications
- Avoiding Infinite Loops: In search spaces where loops are possible, setting a depth limit prevents the search from running indefinitely.
- Constrained Search Problems: When the solution is known to be within a certain depth, DLS efficiently focuses the search within that boundary.
- Component of IDDFS: DLS serves as the underlying mechanism for Iterative Deepening Depth-First Search, where the depth limit is gradually increased to combine the benefits of both breadth-first and depth-first searches.
## Limitations
- The main limitation of DLS is its inability to guarantee completeness or optimality due to the arbitrary depth limit.
- Choosing an appropriate depth limit can be challenging without prior knowledge about the solution's depth.
- DLS may miss solutions located beyond the depth limit or expend effort on paths that don't lead to a solution within the limit.
Depth-Limited Search provides a practical approach to exploring large or infinite search spaces by imposing a manageable boundary on the search depth, making it a valuable tool in the domain of search algorithms.
