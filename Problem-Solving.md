# Chapter 3: Problem Solving

# Chapter 3.1: Summary - Problem-Solving Agents

- **Problem-Solving Agents**
  - Autonomous entities that operate in an environment to achieve goals.

- **Problem Formulation**
  - Identifying and defining problems to be solved.
  - Explicitly specifying the state space, initial state, actions, and goal test.

- **Search Strategies**
  - Systematic exploration of state space to find a solution.
  - Uninformed (blind) search and informed (heuristic) search strategies.

- **Problem Types**
  - Deterministic and nondeterministic problems.
  - Single-agent and multi-agent problems.

- **Problem Reduction**
  - Transforming complex problems into simpler ones.
  - Breaking down problems into subproblems for easier solution.

- **Solution Quality**
  - Evaluating and comparing solutions based on various criteria.
  - Optimality and completeness of solutions.

- **Feedback and Adaptation**
  - Learning from feedback to improve problem-solving capabilities.
  - Adaptive agents adjust strategies based on experience.

- **Challenges and Considerations**
  - Dealing with uncertainties, partial observability, and dynamic environments.
  - Balancing exploration and exploitation in search strategies.

# Chapter 3.2 Summary: Example Problems

## Overview
- Explores various example problems within the problem-solving approach.
- Distinguishes between standardized and real-world problems.

## Standardized Problems

### Grid World
- **Problem Description:**
  - 2D rectangular array of cells.
  - Agent moves horizontally, vertically, or diagonally.
- **Example:**
  - Vacuum world as a grid.
  - Defined states, actions, transition model, goal states, and action costs.

### Sokoban Puzzle
- **Problem Description:**
  - Agent pushes boxes to storage.
  - Constraints on box movement.
- **Example:**
  - Formulated as a grid world problem.
  - Complexity increases with cells and boxes.

### Sliding-Tile Puzzle (8-Puzzle)
- **Problem Description:**
  - Tiles in a grid with one blank space.
  - Goal: Reach a specified configuration.
- **Example:**
  - 8-puzzle with states, actions, transition model, goal states, and action costs.

### Knuth's Infinite State Space Problem
- **Problem Description:**
  - Conjectured problem exploring infinite state spaces.
  - Sequence of operations to reach any positive integer.
- **Example:**
  - Defined states, actions, transition model, goal states, and action costs.

## Real-World Problems

### Route-Finding
- **Problem Description:**
  - Optimal paths between locations with varying costs.
  - Applications in web sites, in-car systems, etc.
- **Example:**
  - States, actions, transition model, goal states, and complex action costs defined.

### Touring Problem (Traveling Salesperson Problem)
- **Problem Description:**
  - Vis


# Chapter 3.3 Summary: Search Algorithms

## Overview
Chapter 3.3 delves into search algorithms that utilize a search tree superimposed over the state space graph. Various paths from the initial state are explored in an attempt to find a solution leading to the goal state. This summary includes an overview of best-first search, search data structures, redundant paths, and performance metrics.

## Best-First Search (Graph Search)
- **Definition:**
  - A search algorithm that chooses nodes based on an evaluation function, f(n), where n is a node.
  - Nodes are expanded and child nodes are generated.
  - Nodes are added to the frontier based on their evaluation function values.
- **Algorithm:**
  - `BEST-FIRST-SEARCH(problem, f)` returns a solution node or failure.
  - `EXPAND(problem, node)` yields nodes.
- **Data Structures:**
  - `NODE`: Represents a node in the search tree with STATE, PARENT, ACTION, and PATH-COST.
  - `Frontier`: Implemented as a priority queue, FIFO queue, or LIFO queue.
  - `Reached`: A lookup table to store previously reached states.
 
  ### Best-First Search (Graph Search) Code

```python
def BEST_FIRST_SEARCH(problem, f):
    node = NODE(STATE=problem.INITIAL)
    frontier = priority_queue_ordered_by_f_with_node_as_element(node)
    reached = {problem.INITIAL: node}

    while not IS_EMPTY(frontier):
        node = POP(frontier)
        if problem.IS_GOAL(node.STATE):
            return node
        for child in EXPAND(problem, node):
            s = child.STATE
            if s not in reached or child.PATH_COST < reached[s].PATH_COST:
                reached[s] = child
                add_child_to_frontier(child, frontier)

    return failure

def EXPAND(problem, node):
    s = node.STATE
    for action in problem.ACTIONS(s):
        s_0 = problem.RESULT(s, action)
        cost = node.PATH_COST + problem.ACTION_COST(s, action, s_0)
        yield NODE(STATE=s_0, PARENT=node, ACTION=action, PATH_COST=cost)
```
![](https://www3.cs.stonybrook.edu/~skiena/combinatorica/animations/anim/bfs.gif)

Source : https://www3.cs.stonybrook.edu/~skiena/combinatorica/animations/search.html 
## Search Data Structures
- **Nodes:**
  - STATE: The state corresponding to the node.
  - PARENT: The parent node that generated the current node.
  - ACTION: The action applied to the parent's state.
  - PATH-COST: The total cost of the path from the initial state to the current node.
- **Frontier:**
  - Implemented as a priority queue, FIFO queue, or LIFO queue.

## Redundant Paths
- **Definition:**
  - Paths that lead to the same state but may have different lengths or sequences.
  - Cycles are a special case of redundant paths.
- **Approaches:**
  - Remember all reached states and keep only the best path to each state.
  - Do not check for repeating the past in formulations where it is rare or impossible.
  - Compromise by checking for cycles but not for redundant paths in general.

## Measuring Problem-Solving Performance
- **Completeness:**
  - Guarantees finding a solution when one exists and correctly reports failure when not.
- **Cost Optimality:**
  - Finds a solution with the lowest path cost among all solutions.
- **Time Complexity:**
  - Measured in seconds or abstractly by considering the number of states and actions.
- **Space Complexity:**
  - Amount of memory needed to perform the search.

## Key Metrics
- **Systematic Search:**
  - A systematic exploration of the state space ensures completeness.
- **Time and Space Complexity:**
  - Measured in terms of the state-space graph size (|V| + |E|).
- **Implicit Depth State Space:**
  - Complexity measured in terms of depth (d), maximum path length (m), and branching factor (b).


## 3.4 Uninformed Search Strategies
### 3.4.1 Breadth-first Search
- Systematic search strategy expanding nodes at the same depth level.
- Complete and optimal for problems with uniform action costs.
- Utilizes a first-in-first-out (FIFO) queue for efficiency.

### 3.4.2 Uniform-cost Search (Dijkstra's Algorithm)
- Extends breadth-first search for problems with varying action costs.
- Ensures optimality by considering paths with lower costs first.
- Cost-complexity based on the optimal solution cost.

### 3.4.3 Depth-first Search
- Expands the deepest node in the frontier first.
- Inefficient for problems with large state spaces.
- Not cost-optimal and may not find the minimum path.

### 3.4.4 Depth-limited and Iterative Deepening Search
- Depth-limited search sets a depth limit for depth-first search.
- Iterative deepening search repeatedly applies depth-limited search with increasing limits.
- Combines benefits of depth-first and breadth-first search.

### 3.4.5 Bidirectional Search
- Simultaneously searches forward from the initial state and backward from the goal state(s).
- Aims to reduce the search space by meeting in the middle.
- Utilizes two frontiers and two tables of reached states.

# Chapter 3.5: Informed (Heuristic) Search Strategies

- **Informed Search Strategies**
  - Use domain-specific hints (heuristics) for goal locations.
  - Heuristic function: h(n) estimates the cost of the cheapest path to a goal state.

- **Heuristic Function**
  - Represents the estimated cost from the state at node n to a goal state.
  - Example: Straight-line distance on the map for route-finding problems.

- **Greedy Best-First Search**
  - Expands nodes based on the lowest h(n) value.
  - Evaluation function: f(n) = h(n).

- **Straight-Line Distance Heuristic (hSLD)**
  - Estimates distances to Bucharest in Romanian route-finding problems.
  - Greedy best-first search with hSLD finds solutions efficiently but may not be optimal.

- **A* Search**
  - Common informed search algorithm.
  - Evaluation function: f(n) = g(n) + h(n).

- **Optimality and Admissibility**
  - A* search is cost-optimal with an admissible heuristic.
  - Admissible heuristic never overestimates the cost to reach a goal.

- **Consistency and Triangle Inequality**
  - Consistent heuristic follows the triangle inequality.
  - Heuristic h(n) ≤ c(n, a, n0) + h(n0) for all nodes and successors.

- **Implementation Considerations**
  - Inconsistent heuristics may lead to complications.
  - Some implementations update successors for better paths to avoid issues.

- **Illustrative Example**
  - Route-finding problem in Romania demonstrates the efficiency and optimality of heuristic search strategies.


# Chapter Summary: Search Algorithms in Environments

This chapter introduces search algorithms applicable to episodic, single-agent, fully observable, deterministic, static, discrete, and completely known environments. Key tradeoffs exist between search time, memory usage, and solution quality.

## Problem Formulation
- Before initiating a search, a well-defined problem must be formulated.
- A problem comprises the initial state, action set, transition model, goal states, and action cost function.

## State Space Representation
- The environment is represented by a state space graph, with a solution being a path from the initial state to a goal state.

## Search Algorithms
- Search algorithms treat states and actions as atomic.
- Judged on completeness, cost optimality, time complexity, and space complexity.

### Uninformed Search Methods
- Access only the problem definition.
- Algorithms differ in node expansion strategies:
  - Best-first search uses an evaluation function.
  - Breadth-first search expands shallowest nodes.
  - Uniform-cost search expands lowest path cost nodes.
  - Depth-first search expands deepest unexpanded nodes.
  - Iterative deepening search combines depth-first with increasing depth limits.
  - Bidirectional search expands two frontiers, stopping when they meet.

### Informed Search Methods
- Access heuristic function h(n) estimating solution cost.
- May utilize additional information like pattern databases.
- Algorithms include:
  - Greedy best-first search expands nodes with minimal h(n).
  - A* search expands nodes with minimal f(n) = g(n) + h(n).
  - Bidirectional A* search.
  - IDA* (iterative deepening A* search) addresses space complexity.
  - RBFS (recursive best-first search) and SMA* (simplified memory-bounded A*) are robust, optimal search algorithms with limited memory use.

> Note: Understanding the strengths and limitations of each algorithm is crucial for selecting an appropriate approach based on the problem at hand.
