# Constraint Satisfaction Problems


In which we see how treating states as more than just little black boxes leads to new search methods and a deeper understanding of problem structure.

Chapters 3 and 4 explored the idea that problems can be solved by searching the state space: a graph where the nodes are states and the edges between them are actions. We saw that domain-specific heuristics could estimate the cost of reaching the goal from a given state, but from the point of view of the search algorithm, each state is atomic, or indivisible—a black box with no internal structure. In this chapter, we break open the black box by using a factored representation for each state: a set of variables, each of which has a value. A problem is solved when each variable has a value that satisfies all the constraints on the variable. A problem described this way is called a **constraint satisfaction problem**, or CSP.

### 5.1 Defining Constraint Satisfaction Problems

A constraint satisfaction problem consists of three components, X, D, and C:

- **X** is a set of variables, `{X1,...,Xn}`.
- **D** is a set of domains, `{D1,...,Dn}`, one for each variable.
- **C** is a set of constraints that specify allowable combinations of values.

A domain, Di, consists of a set of allowable values, `{v1,..., vk}`, for variable Xi. Each constraint Cj consists of a pair `<scope, rel>`, where scope is a tuple of variables that participate in the constraint and rel is a relation that defines the values that those variables can take on.

#### Example problem: Map coloring

Suppose that, having tired of Romania, we are looking at a map of Australia showing each of its states and territories. We are given the task of coloring each region either red, green, or blue in such a way that no two neighboring regions have the same color.

- **Variables (X):** `{WA,NT,Q,NSW,V,SA,T}`
- **Domains (D):** Each variable has the domain `{red,green,blue}`.
- **Constraints (C):** Neighboring regions must have distinct colors.

#### Example problem: Job-shop scheduling

Factories have the problem of scheduling a day’s worth of jobs, subject to various constraints. Consider the problem of scheduling the assembly of a car, composed of tasks such as installing axles, affixing wheels, tightening nuts for each wheel, affixing hubcaps, and inspecting the final assembly.

####  Variations on the CSP formalism

CSPs can involve variables that have discrete or continuous domains. Constraints can be unary (affecting one variable), binary (affecting two variables), or global (affecting an arbitrary number of variables).

- **Discrete and finite domains:** Common in problems like map coloring and scheduling with time limits.
- **Continuous domains:** Common in operations research and include scheduling problems with precise timing.

CSPs with preferences (or preference constraints) indicate which solutions are preferred, allowing for the encoding of costs on individual variable assignments. This formulation enables solving CSPs with optimization search methods, resulting in a **constrained optimization problem (COP)**.


### 5.2 Constraint Propagation: Inference in CSPs

An atomic state-space search algorithm makes progress in only one way: by expanding a node to visit the successors. A CSP algorithm has choices. It can generate successors by choosing a new variable assignment, or it can do a specific type of inference called **constraint propagation**: using the constraints to reduce the number of legal values for a variable, which in turn can reduce the legal values for another variable, and so on.

#### Local Consistency

The key idea is **local consistency**. If we treat each variable as a node in a graph and each binary constraint as an edge, then the process of enforcing local consistency in each part of the graph causes inconsistent values to be eliminated throughout the graph.

####  Node Consistency

A single variable is **node-consistent** if all the values in the variable’s domain satisfy the variable’s unary constraints. For example, making SA node consistent by eliminating green, leaving SA with the reduced domain `{red,blue}`.

####  Arc Consistency

A variable in a CSP is **arc-consistent** if every value in its domain satisfies the variable’s binary constraints. The most popular algorithm for enforcing arc consistency is called **AC-3**.

```plaintext
function AC-3(csp) returns false if an inconsistency is found and true otherwise
    queue←a queue of arcs, initially all the arcs in csp
    while queue is not empty do
        (Xi, Xj)←POP(queue)
        if REVISE(csp, Xi, Xj) then
            if size of Di = 0 then return false
            for each Xk in Xi.NEIGHBORS - {Xj} do
                add (Xk, Xi) to queue
    return true
```

####  Path Consistency
Path consistency tightens the binary constraints by using implicit constraints that are inferred by looking at triples of variables.

####  K-consistency
A CSP is k-consistent if, for any set of k−1 variables and for any consistent assignment to those variables, a consistent value can always be assigned to any kth variable.

#### Global Constraints
Global constraints involve an arbitrary number of variables. Examples include Alldiff, which says that all the variables involved must have distinct values.

###  Sudoku
The popular Sudoku puzzle is a CSP with 81 variables, one for each square, where the domain of each variable is the digits 1 to 9, and there are 27 Alldiff constraints.


# 5.3 Backtracking Search for CSPs

Sometimes we can finish the constraint propagation process and still have variables with multiple possible values. In that case, we have to search for a solution. This section covers backtracking search algorithms that work on partial assignments; in the next section, we look at local search algorithms over complete assignments.

## Commutativity

A problem is commutative if the order of application of any given set of actions does not matter. In CSPs, it makes no difference if we first assign `NSW = red` and then `SA = blue`, or the other way around.

## Backtracking Algorithm

```plaintext
function BACKTRACKING-SEARCH(csp) returns a solution or failure
  return BACKTRACK(csp, { })

function BACKTRACK(csp, assignment) returns a solution or failure
  if assignment is complete then return assignment
  var←SELECT-UNASSIGNED-VARIABLE(csp, assignment)
  for each value in ORDER-DOMAIN-VALUES(csp, var, assignment) do
    if value is consistent with assignment then
      add {var = value} to assignment
      inferences←INFERENCE(csp, var, assignment)
      if inferences 6= failure then
        add inferences to csp
        result←BACKTRACK(csp, assignment)
        if result 6= failure then return result
        remove inferences from csp
        remove {var = value} from assignment
  return failure
```

- The BACKTRACKING-SEARCH algorithm models recursive depth-first search.
- SELECT-UNASSIGNED-VARIABLE and ORDER-DOMAIN-VALUES implement general-purpose heuristics.
- The INFERENCE function can optionally impose arc-, path-, or k-consistency.

###  Variable and Value Ordering
- Minimum-Remaining-Values (MRV) Heuristic: Chooses the variable with the fewest legal values.
- Degree Heuristic: Selects the variable that is involved in the largest number of constraints on other unassigned variables.
- Least-Constraining-Value: Prefers the value that rules out the fewest choices for the neighboring variables.
###  Interleaving Search and Inference
- Forward Checking: Establishes arc consistency for each unassigned variable connected by a constraint to the recently assigned variable.
- Maintaining Arc Consistency (MAC): Calls AC-3 to maintain arc consistency after each assignment.
### Intelligent Backtracking: Looking Backward
- Chronological Backtracking: Revisits the most recent decision point when a branch of the search fails.
- Conflict-Directed Backjumping: Backtracks to a variable that might fix the problem by considering a set of assignments that are in conflict.
### Constraint Learning
- Constraint Learning: Finds a minimum set of variables from the conflict set that causes the problem, called a no-good, and records this combination to avoid encountering the same problem again.
This structure uses Markdown formatting to clearly separate and organize sections, enhancing the readability and understanding of the backtracking search approach for CSPs.


# 5.4 Local Search for CSPs

Local search algorithms are very effective in solving many CSPs by using a complete-state formulation. An example is the 8-queens problem, defined on page 167. 

## Min-conflicts Heuristic

- Initially, a complete assignment violates several constraints.
- A conflicted variable is randomly chosen (e.g., Q8 in the 8-queens problem).
- The goal is to select a value that results in the minimum number of conflicts with other variables.

For the n-queens problem, min-conflicts' runtime is roughly independent of problem size, solving even the million-queens problem in an average of 50 steps.

## Applications

- Min-conflicts has been used to schedule observations for the Hubble Space Telescope, reducing scheduling time from three weeks to around 10 minutes.

## Local Search Techniques for CSPs

- Plateau search and simulated annealing can help find solutions on the landscape of a CSP.
- Tabu search forbids returning to recently visited states to direct wandering on the plateau.
- Constraint weighting concentrates the search on important constraints by giving a numeric weight to each constraint.

## Min-conflicts Algorithm

```plaintext
function MIN-CONFLICTS(csp, max steps) returns a solution or failure
  current ← an initial complete assignment for csp
  for i = 1 to max steps do
    if current is a solution for csp then return current
    var ← a randomly chosen conflicted variable from csp.VARIABLES
    value ← the value v for var that minimizes CONFLICTS(csp, var, v, current)
    set var = value in current
  return failure
```

This algorithm selects a conflicted variable and changes it to the value that minimizes conflicts, iterating until a solution is found or the maximum number of steps is reached.


### Online Setting for Local Search
Local search can be advantageous in an online setting, such as adjusting an airline’s weekly flights schedule in response to unexpected events like bad weather. Starting from the current schedule, local search can efficiently make minimal changes to repair the schedule.

## 5.5 The Structure of Problems

In this section, we discuss how the structure of the problem, represented by the constraint graph, can aid in quickly finding solutions. These approaches are applicable not only to CSPs but also to other areas like probabilistic reasoning.

### Independence and Connected Components

- **Independence**: The ability to decompose the real world into subproblems.
- **Example**: In the Australia map coloring problem, Tasmania and the mainland are independent subproblems. This is observed by identifying connected components in the constraint graph.

#### Connected Component
- A **connected component** refers to a segment of the constraint graph where every variable is connected, indicating a subproblem (`CSPi`). Solutions to these subproblems can be combined to solve the overall problem.

### Graph Structures and Solutions

- Certain graph structures, like a tree, allow for a CSP to be solved linearly in the number of variables.
- **Directional Arc Consistency (DAC)**: A form of consistency important for solving tree-structured CSPs efficiently.

#### Tree CSP Solver Algorithm

```plaintext
function TREE-CSP-SOLVER(csp) returns a solution or failure
  n ← number of variables in X
  assignment ← an empty assignment
  root ← any variable in X
  X ← TOPOLOGICALSORT(X, root)
  for j = n down to 2 do
    MAKE-ARC-CONSISTENT(PARENT(Xj), Xj)
    if it cannot be made consistent then return failure
  for i = 1 to n do
    assignment[Xi] ← any consistent value from Di
    if there is no consistent value then return failure
  return assignment
```

- Topological Sort: An ordering of the variables such that each variable appears after its parent in the tree structure.

### Reducing Constraint Graphs to Trees
There are two methods for reducing a constraint graph to a tree:

#### Cutset Conditioning:
- Involves assigning values to some variables so that the remaining variables form a tree.
- Cycle Cutset: A subset of variables whose assignment results in the constraint graph becoming a tree.
#### Tree Decomposition:
- Transforms the original graph into a tree where each node represents a set of variables.
**Requirements**:
- Every variable appears in at least one node.
- If two variables have a constraint, they must appear together in at least one node.
- If a variable appears in two nodes, it must appear in all nodes along the path connecting those nodes.

### Value Symmetry and Symmetry-Breaking
- **Value Symmetry**: The existence of multiple equivalent solutions that only differ by the permutation of values.
- **Symmetry-Breaking Constraint**: An additional constraint introduced to reduce the search space by eliminating equivalent solutions.
This section highlights the importance of utilizing the problem structure and advanced strategies like cutset conditioning, tree decomposition, and symmetry-breaking to efficiently solve CSPs and similar complex problems.

## Summary
- **Constraint satisfaction problems** (CSPs) represent a state with a set of variable/value
pairs and represent the conditions for a solution by a set of constraints on the variables.
Many important real-world problems can be described as CSPs.
- A number of **inference** techniques use the constraints to rule out certain variable assignments. These include node, arc, path, and k-consistency.
- **Backtracking search**, a form of depth-first search, is commonly used for solving CSPs.
Inference can be interwoven with search.
- The **minimum-remaining-values** and **degree** heuristics are domain-independent methods for deciding which variable to choose next in a backtracking search. The **leastconstraining-value** heuristic helps in deciding which value to try first for a given
variable. Backtracking occurs when no legal assignment can be found for a variable.
**Conflict-directed backjumping** backtracks directly to the source of the problem. **Constraint learning** records the conflicts as they are encountered during search in order to
avoid the same conflict later in the search.

- Local search using the **min-conflicts** heuristic has also been applied to constraint satisfaction problems with great success.
- The complexity of solving a CSP is strongly related to the structure of its constraint
graph. Tree-structured problems can be solved in linear time. **Cutset conditioning** can
reduce a general CSP to a tree-structured one and is quite efficient (requiring only linear memory) if a small cutset can be found. **Tree decomposition** techniques transform
the CSP into a tree of subproblems and are efficient if the **tree width** of the constraint
graph is small; however they need memory exponential in the tree width of the constraint graph. Combining cutset conditioning with tree decomposition can allow a better
tradeoff of memory versus time.

