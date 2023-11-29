# Search Algorithms

#### Objective of Search Algorithm:

- Takes a search problem as input.
- Aims to return a solution or indicate failure.
#### Algorithm Structure:

- Utilizes a search tree superimposed over the state space graph.
- Forms paths from the initial state to find a solution in the goal state.
#### Node and State Correspondence:

- Each node in the search tree corresponds to a state in the state space.
- Edges in the search tree represent actions taken to transition between states.
- The root of the tree corresponds to the initial state of the problem.
#### Understanding State Space and Search Tree:

- Distinction between state space and search tree is crucial.
- State space encompasses the set of states in the world and possible transitions.
- Search tree describes unique paths between states, reaching toward the goal.
#### Search Tree Characteristics:

- May have multiple paths to a given state.
- Each node in the tree has a unique path back to the root.

## 1. Best-first search

### Overview
How do we decide which node from the frontier to expand next? A very general approach
is called **best-first search**, in which we choose a node, n, with minimum value of some Best-first search
**evaluation function**, f(n). The psudocode shows the algorithm. On each iteration we choose Evaluation function
a node on the frontier with minimum f(n) value, return it if its state is a goal state, and
otherwise apply EXPAND to generate child nodes. Each child node is added to the frontier
if it has not been reached before, or is re-added if it is now being reached with a path that
has a lower path cost than any previous path. The algorithm returns either an indication of
failure, or a node that represents a path to a goal. By employing different f(n) functions, we
get different specific algorithms, which this chapter will cover. 
### Psudocode

    function BEST-FIRST-SEARCH(problem, f) returns a solution node or failure
        node←NODE(STATE=problem.INITIAL)
        frontier←a priority queue ordered by f , with node as an element
        reached←a lookup table, with one entry with key problem.INITIAL and value node
        **while not** IS-EMPTY(frontier) do
            node←POP(frontier)
            if problem.IS-GOAL(node.STATE) then return node
            for each child in EXPAND(problem, node) do
            s←child.STATE
            if s is not in reached or child.PATH-COST < reached[s].PATH-COST then
                reached[s]←child
                add child to frontier
    return failure
    
    function EXPAND(problem, node) yields nodes
        s←node.STATE
        for each action in problem.ACTIONS(s) do
            s'←problem.RESULT(s, action)
            cost←node.PATH-COST + problem.ACTION-COST(s, action,s')
            yield NODE(STATE=s', PARENT=node, ACTION=action, PATH-COST=cost)


### Demo
![](https://blog.finxter.com/wp-content/uploads/2021/12/Best-first-search.gif)

## 2.  Breadth-first search

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

## 3. Dijkstra’s algorithm or uniform-cost search

- When actions have different costs, an obvious choice is to use best-first search where the
evaluation function is the cost of the path from the root to the current node. This is called Dijkstra’s algorithm by the theoretical computer science community, and uniform-cost search Uniform-cost search
by the AI community
- The idea is that while breadth-first search spreads out in waves of uniform depth—first depth 1, then depth 2, and so on—uniform-cost search spreads out in waves
of uniform path-cost. The algorithm can be implemented as a call to BEST-FIRST-SEARCH
with PATH-COST as the evaluation function

### Psudocode
	    function Dijkstra(Graph, source):
    	for each vertex v in Graph:	// Initialization
        	dist[v] := infinity	// initial distance from source to vertex v is set to infinite
        	previous[v] := undefined	// Previous node in optimal path from source
    	dist[source] := 0	// Distance from source to source
    	Q := the set of all nodes in Graph	// all nodes in the graph are unoptimized - thus are in Q
    	while Q is not empty:	// main loop
        	u := node in Q with smallest dist[ ]
        	remove u from Q
            for each neighbor v of u:	// where v has not yet been removed from Q.
            	alt := dist[u] + dist_between(u, v)
                if alt < dist[v]	// Relax (u,v)
                	dist[v] := alt
        	        previous[v] := u
        return previous[ ]

### Demo 
![image](https://github.com/NoumanAMalik/i-love-ai-docs/assets/63931307/3648fa29-44a6-4f61-bdec-c8e72cf15e5f)

## 4. Depth-limited search

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


## 5. Bidirectional search
bidirectional search simultaneously
searches forward from the initial state and backwards from the goal state(s), hoping that the
two searches will meet. The motivation is that b
d/2 +b
d/2
is much less than b
d
(e.g., 50,000
times less when b=d =10).



### Psudocode

function BIBF-SEARCH(problemF, fF, problemB, fB) returns a solution node, or failure
    nodeF ←NODE(problemF.INITIAL) // Node for a start state
    nodeB←NODE(problemB.INITIAL) // Node for a goal state
    frontierF ←a priority queue ordered by fF, with nodeF as an element
    frontierB←a priority queue ordered by fB, with nodeB as an element
    reachedF ←a lookup table, with one key nodeF.STATE and value nodeF
    reachedB←a lookup table, with one key nodeB.STATE and value nodeB
    solution←failure
    while not TERMINATED(solution, frontierF, frontierB) do
        if fF(TOP(frontierF)) < fB(TOP(frontierB)) then
            solution←PROCEED(F, problemF, frontierF, reachedF, reachedB, solution)
        else solution←PROCEED(B, problemB, frontierB, reachedB, reachedF, solution)
return solution

function PROCEED(dir, problem, frontier, reached, reached2, solution) returns a solution
        // Expand node on frontier; check against the other frontier in reached2.
        // The variable “dir” is the direction: either F for forward or B for backward.
    node←POP(frontier)
        for each child in EXPAND(problem, node) do
        s←child.STATE
        if s not in reached or PATH-COST(child) < PATH-COST(reached[s]) then
            reached[s]←child
            add child to frontier
            if s is in reached2 then
                solution2←JOIN-NODES(dir, child, reached2[s]))
                if PATH-COST(solution2) < PATH-COST(solution) then
                solution←solution2
return solution

### Demo 
![image](https://github.com/NoumanAMalik/i-love-ai-docs/assets/63931307/a76a9d12-413a-40fb-bd85-64f75b2125eb)

## 6. Greedy best-first search
- Greedy best-first search is an informed search algorithm where the evaluation function is strictly equal to the heuristic function, disregarding the edge weights in a weighted graph because only the heuristic value is considered. In order to search for a goal node it expands the node that is closest to the goal as determined by the heuristic function. This approach assumes that it is likely to lead to a solution quickly. However, the solution from a greedy best-first search may not be optimal since a shorter path may exist.

- In this algorithm, search cost is at a minimum since the solution is found without expanding a node that is not on the solution path. This algorithm is minimal, but not complete, since it can lead to a dead end. It’s called “Greedy” because at each step it tries to get as close to the goal as it can.
### Psudocode
- Greedy Best-First Search works by evaluating the cost of each possible path and then expanding the path with the lowest cost. This process is repeated until the goal is reached. 
- The algorithm uses a heuristic function to determine which path is the most promising. 
- The heuristic function takes into account the cost of the current path and the estimated cost of the remaining paths. 
- If the cost of the current path is lower than the estimated cost of the remaining paths, then the current path is chosen. This process is repeated until the goal is reached.

### Demo 

![](https://upload.wikimedia.org/wikipedia/commons/f/f9/Greedy-search-path.gif)
## 7. A∗ search
- A* Search algorithm is one of the best and popular technique used in path-finding and graph traversals
- Informally speaking, A* Search algorithms, unlike other traversal techniques, it has “brains”. What it means is that it is really a smart algorithm which separates it from the other conventional algorithms.

The most common informed search algorithm is A
∗
search (pronounced “A-star search”), a A
∗
search
best-first search that uses the evaluation function
f(n) = g(n) +h(n)
where g(n) is the path cost from the initial state to node n, and h(n) is the estimated cost of
the shortest path from n to a goal state, so we have

f(n) = estimated cost of the best path that continues from n to a goal.
### Demo 

![](https://i.imgur.com/YjfzxXY.gif)


## 8. Iterative deepening search

- Iterative deepening search solves the problem of picking a good value for ` by trying Iterative deepening
search
all values: first 0, then 1, then 2, and so on—until either a solution is found, or the depthlimited search returns the failure value rather than the cutoff value.
- Iterative deepening combines many of the benefits of depth-first and breadth-first
search. Like depth-first search, its memory requirements are modest: O(bd) when there is a
solution, or O(bm) on finite state spaces with no solution.
- Like breadth-first search, iterative
deepening is optimal for problems where all actions have the same cost, and is complete on
finite acyclic state spaces, or on any finite state space when we check nodes for cycles all the
way up the path.

### Psudocode
      function ITERATIVE-DEEPENING-SEARCH(problem) returns a solution node or failure
            for depth = 0 to ∞ do
                result←DEPTH-LIMITED-SEARCH(problem, depth)
                if result 6= cutoff then return result

### Demo 

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/iddfs11-1024x420.png)
![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/iddfs2-1024x420.png)
