# Chapter 3: Solving Problems by Searching

This chapter explores how agents can plan a series of actions to achieve their goals, focusing on the concept of *problem-solving agents* and the process known as *search*.

- **Problem-solving Agents**: These agents use atomic representations of the world, meaning they see states as wholes without internal structure. This contrasts with planning agents, which use more complex state representations.
- **Search Algorithms**: The chapter covers various algorithms for navigating through problems, distinguishing between informed and uninformed strategies based on whether the agent can estimate its proximity to the goal.
- **Environments**: Discussion is limited to simple environments, characterized as episodic, single-agent, fully observable, deterministic, static, discrete, and known. Later chapters expand on these constraints.
- **Key Concepts**: Introduces asymptotic complexity (O(n) notation), essential for understanding the efficiency of different search strategies.

For those new to asymptotic complexity, it's recommended to consult Appendix A for a deeper understanding.

## 3.1 Problem-Solving Agents

Imagine an agent on vacation in Romania, facing the complex problem of navigating to Bucharest without direct knowledge of the area. This scenario illustrates the essence of problem-solving agents in AI, which undertake a search process to achieve goals.

### Key Steps in Problem-Solving:

1. **Goal Formulation**: Define the end state. For our agent, it's reaching Bucharest.
2. **Problem Formulation**: Create an abstract model of the problem, focusing on relevant aspects like possible actions and their outcomes.
3. **Search**: Simulate action sequences in the model to find a solution path.
4. **Execution**: Carry out the actions in the real world.

This process, while simplified, highlights how agents deal with challenges in controlled environments, paving the way for more complex scenarios discussed in subsequent chapters.

## Chapter 3.2: Example Problems

This section explores the application of problem-solving approaches to a broad spectrum of environments, differentiating between standardized problems, which serve as benchmarks for algorithm performance comparison, and real-world problems that are directly utilized and are more idiosyncratic in nature.

## Standardized Problems

A standardized problem is intended to illustrate or exercise various problem-solving methods. It can be given a concise, exact description and hence is suitable as a benchmark for researchers to compare the performance of algorithms.

- **Grid World**: Focuses on a 2D array of cells where agents move, interact with objects, or face obstacles. Example: Vacuum world.
- **Sliding-Tile Puzzle**: Involves arranging tiles within a grid to achieve a specific configuration. Variants include the 8-puzzle and the 15-puzzle.
- **Infinite State Spaces**: Demonstrated by problems like the one devised by Donald Knuth involving operations on the number 4 to reach any positive integer.

### Standardized Problems in Depth

### The Vacuum World

- **States**: Describes the location of objects within cells, including the agent and any dirt. For a simple two-cell version, there are eight possible states. In a more complex environment with `n` cells, the total states are `n * 2^n`.
- **Actions**: Include Suck, Left, Right, and potentially more for larger worlds, like Upward, Downward, or egocentric actions (Forward, Backward, TurnRight, TurnLeft).
- **Transition Model**: Describes the outcome of actions, like Suck removing dirt or Forward moving the agent unless obstructed.
- **Goal States**: Achieved when every cell is clean.
- **Action Cost**: Fixed cost of 1 for each action.

### Sokoban Puzzle

- **Objective**: Push boxes to designated storage locations without moving them into other boxes or walls. The complexity increases significantly with the number of cells and boxes.

### Sliding-Tile Puzzle (8-puzzle and 15-puzzle)

- **States**: Each tile's location.
- **Actions**: Move the blank space in four directions, considering the edges and corners.
- **Transition Model**: Swaps the tile and the blank space based on the chosen action.
- **Goal State**: Tiles arranged in a specific order.
- **Action Cost**: Fixed at 1 for all moves.

### Knuth's Conjecture on Infinite State Spaces

- **States**: Positive real numbers.
- **Initial State**: The number 4.
- **Actions**: Square root, floor, or factorial (for integers only).
- **Transition Model**: Follows mathematical operations.
- **Goal State**: A specific positive integer.
- **Action Cost**: Each operation costs 1.

This formulation showcases the varied complexity and design considerations for creating and solving standardized problems within AI and computational problem-solving domains.


## Real-World Problems

A real-world problem, such as robot
navigation, is one whose solutions people actually use, and whose formulation is idiosyncratic, not standardized, because, for example, each robot has different sensors that produce
different data.

- **Route-Finding**: Utilized in applications ranging from driving directions to network routing and military planning.
- **Touring Problems**: Involves visiting a set of locations, notably exemplified by the Traveling Salesperson Problem (TSP).
- **VLSI Layout**: Concerns the placement of millions of components on a chip to optimize various performance metrics.
- **Robot Navigation**: Extends route-finding to more complex and less structured environments, involving many dimensions of movement.
- **Automatic Assembly Sequencing**: Used in industries for the assembly of complex objects, focusing on optimizing assembly order to minimize manual labor.

These examples illustrate the diversity of problems that can be approached with problem-solving strategies, highlighting the adaptability of these methods to both abstract benchmarks and practical applications.

# 3.3 Search Algorithms

A search algorithm takes a search problem as input and returns a solution, or an indication of failure. In this chapter, we consider algorithms that superimpose a search tree over the state-space graph, forming various paths from the initial state, trying to find a path that reaches a goal state. Each node in the search tree corresponds to a state in the state space, and the edges in the search tree correspond to actions. The root of the tree corresponds to the initial state of the problem.

It is important to understand the distinction between the state space and the search tree. The state space describes the (possibly infinite) set of states in the world, and the actions that allow transitions from one state to another. The search tree describes paths between these states, reaching towards the goal. The search tree may have multiple paths to (and thus multiple nodes for) any given state, but each node in the tree has a unique path back to the root (as in all trees).

## Figures

- **Figure 3.4** shows the first few steps in finding a path from Arad to Bucharest.
- **Figure 3.5** A sequence of search trees generated by a graph search on the Romania problem.
- **Figure 3.6** The separation property of graph search, illustrated on a rectangular-grid problem.

## Expanding Nodes

- **Expand:** Consider the available ACTIONS for that state, using the RESULT function to see where those actions lead to, and generating a new node (called a child node or successor node) for each of the resulting states. Each child node has Arad as its parent node.

## Frontier and Reached States

- **Frontier:** A set of unexpanded nodes (outlined in bold). We say that any state that has had a node generated for it has been reached (whether or not that node has been expanded).
- **Reached:** States corresponding to these two types of nodes are said to have been reached.

## Best-first Search

```plaintext
function BEST-FIRST-SEARCH(problem, f) returns a solution node or failure
    node←NODE(STATE=problem.INITIAL)
    frontier←a priority queue ordered by f, with node as an element
    reached←a lookup table, with one entry with key problem.INITIAL and value node
    while not IS-EMPTY(frontier) do
        node←POP(frontier)
        if problem.IS-GOAL(node.STATE) then return node
        for each child in EXPAND(problem, node) do
            s←child.STATE
            if s is not in reached or child.PATH-COST < reached[s].PATH-COST then
                reached[s]←child
                add child to frontier
    return failure
```
### 3.3.2 Search Data Structures

Search algorithms require a data structure to keep track of the search tree. A node in the tree is represented by a data structure with four components:

- `node.STATE`: the state to which the node corresponds;
- `node.PARENT`: the node in the tree that generated this node;
- `node.ACTION`: the action that was applied to the parent’s state to generate this node;
- `node.PATH-COST`: the total cost of the path from the initial state to this node. In mathematical formulas, we use `g(node)` as a synonym for `PATH-COST`.

Following the `PARENT` pointers back from a node allows us to recover the states and actions along the path to that node. Doing this from a goal node gives us the solution.

**Queue:** We need a data structure to store the frontier. The appropriate choice is a queue of some kind, because the operations on a frontier are:

- `IS-EMPTY(frontier)` returns true only if there are no nodes in the frontier.
- `POP(frontier)` removes the top node from the frontier and returns it.
- `TOP(frontier)` returns (but does not remove) the top node of the frontier.
- `ADD(node, frontier)` inserts node into its proper place in the queue.

Three kinds of queues are used in search algorithms:

- **Priority queue:** A priority queue first pops the node with the minimum cost according to some evaluation function, `f`. It is used in best-first search.
- **FIFO queue:** A FIFO queue or first-in-first-out queue first pops the node that was added to the queue first; it is used in breadth-first search.
- **LIFO queue:** A LIFO queue or last-in-first-out queue (also known as a stack) pops first the most recently added node; it is used in depth-first search.

The reached states can be stored as a lookup table (e.g., a hash table) where each key is a state and each value is the node for that state.

### 3.3.3 Redundant Paths

The search tree shown in Figure 3.4 (bottom) includes a path from Arad to Sibiu and back to Arad again. We say that Arad is a repeated state in the search tree, generated in this case by a cycle (also known as a loopy path). So even though the state space has only 20 states, the complete search tree is infinite because there is no limit to how often one can traverse a loop.

A cycle is a special case of a redundant path. For example, we can get to Sibiu via the path Arad–Sibiu (140 miles long) or the path Arad–Zerind–Oradea–Sibiu (297 miles long). This second path is redundant—it’s just a worse way to get to the same state—and need not be considered in our quest for optimal paths.

Consider an agent in a 10×10 grid world, with the ability to move to any of 8 adjacent squares. If there are no obstacles, the agent can reach any of the 100 squares in 9 moves or fewer. But the number of paths of length 9 is almost 8^9 (a bit less because of the edges of the grid), or more than 100 million. In other words, the average cell can be reached by over a million redundant paths of length 9, and if we eliminate redundant paths, we can complete a search roughly a million times faster. As the saying goes, "algorithms that cannot remember the past are doomed to repeat it."

There are three approaches to this issue:

1. **First,** we can remember all previously reached states (as best-first search does), allowing us to detect all redundant paths, and keep only the best path to each state. This is appropriate for state spaces where there are many redundant paths, and is the preferred choice when the table of reached states will fit in memory.
2. **Second,** we can not worry about repeating the past. There are some problem formulations where it is rare or impossible for two paths to reach the same state. For those problems, we could save memory space if we don’t track reached states and we don’t check for redundant paths. We call a search algorithm a graph search if it checks for redundant paths and a tree-like search if it does not check.
3. **Third,** we can compromise and check for cycles, but not for redundant paths in general. Since each node has a chain of parent pointers, we can check for cycles with no need for additional memory by following up the chain of parents to see if the state at the end of the path has appeared earlier in the path. Some implementations follow this chain all the way up, and thus eliminate all cycles; other implementations follow only a few links (e.g., to the parent, grandparent, and great-grandparent), and thus take only a constant amount of time, while eliminating all short cycles (and relying on other mechanisms to deal with long cycles).

### 3.3.4 Measuring Problem-Solving Performance

Before we get into the design of various search algorithms, we will consider the criteria used to choose among them. We can evaluate an algorithm’s performance in four ways:

- **Completeness:** Is the algorithm guaranteed to find a solution when there is one, and to correctly report failure when there is not?
- **Cost optimality:** Does it find a solution with the lowest path cost of all solutions?
- **Time complexity:** How long does it take to find a solution? This can be measured in seconds, or more abstractly by the number of states and actions considered.
- **Space complexity:** How much memory is needed to perform the search?


## 3.4 Uninformed Search Strategies

An uninformed search algorithm is given no clue about how close a state is to the goal(s). For example, consider our agent in Arad with the goal of reaching Bucharest. An uninformed agent with no knowledge of Romanian geography has no clue whether going to Zerind or Sibiu is a better first step. In contrast, an informed agent (Section 3.5) who knows the location of each city knows that Sibiu is much closer to Bucharest and thus more likely to be on the shortest path.

###  Breadth-first search

When all actions have the same cost, an appropriate strategy is **breadth-first search**, in which the root node is expanded first, then all the successors of the root node are expanded next, then their successors, and so on. This is a systematic search strategy that is therefore complete even on infinite state spaces. We could implement breadth-first search as a call to `BEST-FIRST-SEARCH` where the evaluation function `f(n)` is the depth of the node—that is, the number of actions it takes to reach the node.

However, we can get additional efficiency with a couple of tricks. A first-in-first-out queue will be faster than a priority queue, and will give us the correct order of nodes: new nodes (which are always deeper than their parents) go to the back of the queue, and old nodes, which are shallower than the new nodes, get expanded first. In addition, `reached` can be a set of states rather than a mapping from states to nodes, because once we’ve reached a state, we can never find a better path to the state. That also means we can do an **early goal test**, checking whether a node is a solution as soon as it is generated, rather than the late goal test that best-first search uses.

**Breadth-first search** always finds a solution with a minimal number of actions, because when it is generating nodes at depth `d`, it has already generated all the nodes at depth `d-1`, so if one of them were a solution, it would have been found. That means it is cost-optimal for problems where all actions have the same cost, but not for problems that don’t have that property. It is complete in either case.

```plaintext
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
```

### Dijkstra’s algorithm or uniform-cost search
When actions have different costs, an obvious choice is to use best-first search where the evaluation function is the cost of the path from the root to the current node. This is called Dijkstra’s algorithm by the theoretical computer science community, and uniform-cost search by the AI community.

### Depth-first search and the problem of memory
Depth-first search always expands the deepest node in the frontier first. It could be implemented as a call to BEST-FIRST-SEARCH where the evaluation function f is the negative of the depth. However, it is usually implemented not as a graph search but as a tree-like search that does not keep a table of reached states.

### Depth-limited and iterative deepening search
To keep depth-first search from wandering down an infinite path, we can use depth-limited search, a version of depth-first search in which we supply a depth limit, \, and treat all nodes at depth \ as if they had no successors. Iterative deepening search solves the problem of picking a good value for \ by trying all values: first 0, then 1, then 2, and so on.

### Bidirectional search
An alternative approach called bidirectional search simultaneously searches forward from the initial state and backwards from the goal state(s), hoping that the two searches will meet. The motivation is that b^(d/2) + b^(d/2) is much less than b^d.

### Comparing uninformed search algorithms
Figure 3.15 compares uninformed search algorithms in terms of the four evaluation criteria set forth in Section 3.3.4.


| Criterion        | Breadth-First | Uniform-Cost      | Depth-First | Depth-Limited | Iterative Deepening | Bidirectional (if applicable) |
|------------------|---------------|-------------------|-------------|---------------|---------------------|-------------------------------|
| Complete?        | Yes1          | Yes1,2            | No          | No            | Yes1                | Yes1,4                        |
| Optimal cost?    | Yes3          | Yes               | No          | No            | Yes3                | Yes3,4                        |
| Time             | O(b^d)        | O(b^(1+bC*/ε))   | O(b^m)      | O(b^`)        | O(b^d)              | O(b^(d/2))                    |
| Space            | O(b^d)        | O(b^(1+bC*/ε))   | O(bm)       | O(b`)         | O(bd)               | O(b^(d/2))                    |



This comparison is for tree-like search versions which don’t check for repeated states. For graph searches which do check, the main differences are that depth-first search is complete for finite state spaces, and the space and time complexities are bounded by the size of the state space (the number of vertices and edges, |V| + |E|).

## 3.5 Informed (Heuristic) Search Strategies

Informed search strategies leverage domain-specific hints about the location of goals to find solutions more efficiently than uninformed strategies. These hints come in the form of a heuristic function, `h(n)`, defined as the estimated cost of the cheapest path from the state at node `n` to a goal state.

### 3.5.1 Greedy Best-first Search

**Greedy best-first search** prioritizes nodes that appear to be closest to the goal based on the lowest `h(n)` value. The evaluation function is `f(n) = h(n)`.

For route-finding problems, a practical example of a heuristic function is the straight-line distance (SLD) from the current state to the goal, referred to as `hSLD`.

#### Figure 3.16: Straight-line Distances to Bucharest

Values of `hSLD` represent the straight-line distances to Bucharest for various Romanian cities.

### 3.5.2 A* Search

The **A\* search** algorithm is a best-first search that uses the evaluation function `f(n) = g(n) + h(n)`, where `g(n)` is the path cost from the initial state to node `n`, and `h(n)` is the estimated cost to reach a goal from `n`.

A\* search is complete and cost-optimal with an admissible heuristic, meaning it never overestimates the cost to reach the goal.

### 3.5.3 Search Contours

Search contours can be visualized similarly to topographic map contours, representing nodes of equal `f` cost. A\* search expands nodes in concentric bands of increasing `f` cost, focusing the search towards the goal.

### 3.5.4 Satisficing Search: Inadmissible Heuristics and Weighted A*

To explore fewer nodes, satisficing search accepts suboptimal solutions that are "good enough." Weighted A\* search uses `f(n) = g(n) + W * h(n)` with `W > 1`, potentially exploring fewer nodes at the risk of missing the optimal path.

### 3.5.5 Memory-bounded Search

Memory-bounded algorithms, like **SMA\***, manage memory usage to conserve space while searching for solutions. SMA\* expands the best leaf until memory is full, then drops the worst leaf to free space, using linear space while retaining the quality of the best path explored.

### 3.5.6 Bidirectional Heuristic Search

Bidirectional heuristic search operates by searching both from the initial state and towards it from the goal, potentially meeting in the middle for efficiency gains. It uses a modified evaluation function to ensure optimality and completeness with admissible heuristics.

#### Example: Bidirectional Search and Search Contours

Bidirectional search can be visualized with search contours indicating nodes of equal `f` cost from both directions, focusing the search and potentially reducing the number of nodes expanded.

This approach to Markdown preserves the structure of the original text, emphasizes key elements through headings, and provides clarity on complex topics like A\* search and its variations.

## 3.6 Heuristic Functions

In this section, we explore the impact of heuristic accuracy on search performance and discuss methods for creating heuristics, with the 8-puzzle serving as our primary example.

### Heuristic Examples for the 15-Puzzle

- **h1**: Number of misplaced tiles (excluding the blank). For instance, in Figure 3.25, h1 = 8 since all eight tiles are misplaced.
- **h2**: Sum of the distances of the tiles from their goal positions, known as the Manhattan distance. For the start state in Figure 3.25, h2 = 18.

#### Figure 3.25: An Instance of the 8-Puzzle

![8-Puzzle Instance](URL_to_Figure_3.25_image)

###  The Effect of Heuristic Accuracy on Performance

The **effective branching factor** `b*` helps characterize a heuristic's quality. It's defined such that `N + 1 = 1 + b* + (b*)^2 + ... + (b*)^d`, where `N` is the total number of nodes generated by A* for a problem and `d` is the solution depth.

#### Table: Search Costs and Effective Branching Factors

| d  | BFS  | A*(h1) | A*(h2) | BFS  | A*(h1) | A*(h2) |
|----|------|--------|--------|------|--------|--------|
| 6  | 128  | 24     | 19     | 2.01 | 1.42   | 1.34   |
| ...| ...  | ...    | ...    | ...  | ...    | ...    |
| 28 |463234|202565  |22055   | 1.53 | 1.49   | 1.36   |

> **Note**: Data averaged over 100 puzzles for each solution length `d` from 6 to 28.

###  Generating Heuristics from Relaxed Problems

A **relaxed problem** simplifies the original problem by removing some constraints, allowing for easier solution cost calculation. The cost of an optimal solution in a relaxed problem serves as an admissible heuristic for the original problem.

###  Generating Heuristics from Subproblems: Pattern Databases

Pattern databases store the exact solution costs for every possible instance of a subproblem, providing an admissible heuristic `hDB` for the original problem by simply looking up the corresponding subproblem configuration in the database.

###  Generating Heuristics with Landmarks

Landmark-based heuristics precompute the optimal path costs from landmarks to other vertices in the graph. The differential heuristic `hDH(n)` is admissible and uses landmark precomputations to estimate the cost to the goal.

###  Learning to Search Better

Metalevel state spaces represent the computational state of a search program, allowing for learning algorithms to improve search strategies based on experience, aiming to minimize the total problem-solving cost.

###  Learning Heuristics from Experience

Machine learning techniques can construct heuristic functions based on past search experiences, solving instances of problems like the 8-puzzle. Features relevant to predicting a state's heuristic value can significantly aid in this learning process.


## Summary

This chapter has introduced search algorithms that an agent can use to select action sequences in a wide variety of environments—as long as they are episodic, single-agent, fully observable, deterministic, static, discrete, and completely known. There are tradeoffs to be made between the amount of time the search takes, the amount of memory available, and the quality of the solution. We can be more efficient if we have domain-dependent knowledge in the form of a heuristic function that estimates how far a given state is from the goal, or if we precompute partial solutions involving patterns or landmarks.

### Key Points

- **Problem Formulation**: Before an agent can start searching, a well-defined problem must be formulated.
- **Problem Components**: A problem consists of five parts: the initial state, a set of actions, a transition model describing the results of those actions, a set of goal states, and an action cost function.
- **State Space Graph**: The environment of the problem is represented by a state space graph. A path through the state space from the initial state to a goal state is a solution.
- **Search Algorithm Properties**: Search algorithms are judged on the basis of completeness, cost optimality, time complexity, and space complexity.

### Uninformed Search Methods

- **Best-first Search**: Selects nodes for expansion using an evaluation function.
- **Breadth-first Search**: Expands the shallowest nodes first; it is complete and optimal for unit action costs but has exponential space complexity.
- **Uniform-cost Search**: Expands the node with the lowest path cost, g(n), and is optimal for general action costs.
- **Depth-first Search**: Expands the deepest unexpanded node first. It is neither complete nor optimal but has linear space complexity.
- **Iterative Deepening Search**: Calls depth-first search with increasing depth limits until a goal is found.
- **Bidirectional Search**: Expands two frontiers, one around the initial state and one around the goal, stopping when the two frontiers meet.

### Informed Search Methods

- **Greedy Best-first Search**: Expands nodes with minimal h(n). It is not optimal but is often efficient.
- **A\* Search**: Expands nodes with minimal f(n) = g(n) + h(n). A\* is complete and optimal, provided that h(n) is admissible.
- **Bidirectional A\* Search**: Sometimes more efficient than A\* itself.
- **IDA\* (Iterative Deepening A\* Search)**: Addresses the space complexity issue of A\*.
- **RBFS (Recursive Best-first Search) and SMA\* (Simplified Memory-bounded A\*)**: Are robust, optimal search algorithms that use limited memory.
- **Beam Search**: Limits the size of the frontier; it is incomplete and suboptimal but often finds reasonably good solutions.
- **Weighted A\* Search**: Focuses the search towards the goal, expanding fewer nodes but sacrificing optimality.

### Heuristic Function Performance

The performance of heuristic search algorithms depends on the quality of the heuristic function. Good heuristics can be constructed by relaxing the problem definition, storing precomputed solution costs for subproblems in a pattern database, defining landmarks, or learning from experience with the problem class.

