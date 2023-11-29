## Breadth-first search

### Overview

- When all actions have the same cost, an appropriate strategy is breadth-first search, in which
the root node is expanded first, then all the successors of the root node are expanded next,
then their successors, and so on.
- This is a systematic search strategy that is therefore complete even on infinite state spaces.
- However, we can get additional efficiency with a couple of tricks. A first-in-first-out
queue will be faster than a priority queue, and will give us the correct order of nodes: new
nodes (which are always deeper than their parents) go to the back of the queue, and old nodes,
which are shallower than the new nodes, get expanded first.
- In addition, reached can be a set of states rather than a mapping from states to nodes, because once we’ve reached a state,
we can never find a better path to the state. That also means we can do an early goal test,
checking whether a node is a solution as soon as it is generated, rather than the late goal test
that best-first search uses, waiting until a node is popped off the queue.


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

### Demo
![](https://aquarchitect.github.io/swift-algorithm-club/Breadth-First%20Search/Images/AnimatedExample.gif)
