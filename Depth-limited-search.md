## Depth-limited search

- To keep depth-first search from wandering down an infinite path, we can use depth-limited
search, a version of depth-first search in which we supply a depth limit, L, and treat all nodes
at depth L as if they had no successors.
- The time complexity is O(b^L) and the space complexity is O(bL)
- Unfortunately, if we make a poor choice for ` the algorithm
will fail to reach the solution, making it incomplete again.
- Since depth-first search is a tree-like search, we can’t keep it from wasting time on redundant paths in general, but we can eliminate cycles at the cost of some computation time.
If we look only a few links up in the parent chain we can catch most cycles; longer cycles are
handled by the depth limit.



### Psudocode

    function DEPTH-LIMITED-SEARCH(problem, `) returns a node or failure or cutoff
        frontier←a LIFO queue (stack) with NODE(problem.INITIAL) as an element
        result←failure
        while not IS-EMPTY(frontier) do
            node←POP(frontier)
            if problem.IS-GOAL(node.STATE) then return node
            if DEPTH(node) > ` then
                result←cutoff
             else if not IS-CYCLE(node) do
                for each child in EXPAND(problem, node) do
                    add child to frontier
    return result

### Demo 
![](https://thealgoristsblob.blob.core.windows.net/thealgoristsimages/dfs.gif)
