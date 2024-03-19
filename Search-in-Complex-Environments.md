
# Search in Complex Environments  

In which we relax the simplifying assumptions of the previous chapter, to get closer to the real world.

Chapter 3 addressed problems in fully observable, deterministic, static, known environments where the solution is a sequence of actions. In this chapter, we relax those constraints. We begin with the problem of finding a good state without worrying about the path to get there, covering both discrete (**Section 4.1**) and continuous (**Section 4.2**) states. Then we relax the assumptions of determinism (**Section 4.3**) and observability (**Section 4.4**). In a nondeterministic world, the agent will need a conditional plan and carry out different actions depending on what it observes—for example, stopping if the light is red and going if it is green. With partial observability, the agent will also need to keep track of the possible states it might be in. Finally, **Section 4.5** guides the agent through an unknown space that it must learn as it goes, using online search.

###  Local Search and Optimization Problems

In the search problems of Chapter 3, we wanted to find paths through the search space, such as a path from Arad to Bucharest. But sometimes we care only about the final state, not the path to get there. For example, in the 8-queens problem (**Figure 4.3**), we care only about finding a valid final configuration of 8 queens (because if you know the configuration, it is trivial to reconstruct the steps that created it). This is also true for many important applications such as integrated-circuit design, factory floor layout, job shop scheduling, automatic programming, telecommunications network optimization, crop planning, and portfolio management.

#### Local Search

Local search algorithms operate by searching from a start state to neighboring states, without keeping track of the paths, nor the set of states that have been reached. That means they are not systematic—they might never explore a portion of the search space where a solution actually resides. However, they have two key advantages: 

1. They use very little memory.
2. They can often find reasonable solutions in large or infinite state spaces for which systematic algorithms are unsuitable.
---
#### Hill Climbing
Hill Climbing is a heuristic search algorithm used for mathematical optimization problems. At its core, it's a strategy to find the peak (maximum or minimum) of a landscape, where the landscape represents the solution space of the problem, and the elevation of any point in this landscape represents the value of the objective function at that point.

#### How It Works:
Start with an arbitrary solution to a problem.
Iteratively make small changes to the solution, each time improving it a little.
If the change leads to a better solution, it is accepted; otherwise, it is rejected.
The process repeats until no further improvements can be found.

#### Applications:

Used in machine learning for training models.
Optimization problems where finding an exact solution is impractical due to the problem's complexity.
Automated planning and scheduling.

- Strengths: Simple to implement and understand. Efficient in finding a local optimum for problems with a single peak.
- Limitations: Can get stuck in local maxima or minima. Does not guarantee finding the global optimum.

---
#### Local Beam Search
Local Beam Search is a variation of the Hill Climbing algorithm that operates with multiple "beams" or states at once instead of a single state. This approach helps in diversifying the search and reduces the chance of getting stuck in a local optimum.

#### How It Works:
Starts with a set of randomly generated states.
At each iteration, it explores all successors of all current states and selects the best 
k states to consider in the next iteration.
The process repeats until a solution is found or a termination condition is met.

#### Applications:

Useful in problems where the search space is vast, and solutions are dense.
Often used in natural language processing and machine learning model configuration.

- Strengths: More effective at avoiding local optima than standard Hill Climbing. Can find solutions more quickly due to parallel exploration.
- Limitations: Requires more memory to store multiple states. The choice of k is crucial for its performance and efficiency.

---
#### Evolutionary Algorithms
Evolutionary Algorithms are inspired by the process of natural selection, mimicking biological evolution to solve optimization and search problems. They work with a population of potential solutions, evolving them over generations.

#### How It Works:
1. Initialization: Start with a randomly generated population of individuals (potential solutions).
2. Selection: Evaluate the fitness of each individual and select the fittest ones for reproduction.
3. Crossover (Recombination): Combine parts of two or more parents to create offspring.
4. Mutation: Introduce random changes to individual solutions.
5. Replacement: Form a new generation of individuals from the offspring and possibly some members of the current generation.
6. Termination: Repeat the process for many generations until a satisfactory solution is found or a predefined condition is met.
   
#### Applications:
Widely used in optimization problems where the solution space is complex and not well understood.
Applications include scheduling, engineering design, machine learning, and artificial life.

- Strengths: Capable of exploring a vast search space. Can find global optima in complex landscapes. Flexible and adaptable to various types of problems.
- Limitations: Can be computationally intensive. Performance depends heavily on the choice of parameters (e.g., population size, mutation rate).

## 4.2 Local Search in Continuous Spaces

In Chapter 2, we explained the distinction between discrete and continuous environments, pointing out that most real-world environments are continuous. A continuous action space has an infinite branching factor, and thus can't be handled by most of the algorithms we have covered so far (with the exception of first-choice hill climbing and simulated annealing). This section provides a very brief introduction to some local search techniques for continuous spaces. The literature on this topic is vast; many of the basic techniques originated in the 17th century, after the development of calculus by Newton and Leibniz. We find uses for these techniques in several places in this book, including the chapters on learning, vision, and robotics.

We begin with an example. Suppose we want to place three new airports anywhere in Romania, such that the sum of squared straight-line distances from each city on the map to its nearest airport is minimized. (See Figure 3.1 for the map of Romania.) The state space is then defined by the coordinates of the three airports: \( (x_1, y_1), (x_2, y_2), and (x_3, y_3) \). This is a six-dimensional space; we also say that states are defined by six variables. In general, states are defined by an n-dimensional vector of variables. Moving around in this space corresponds to moving one or more of the airports on the map. The objective function \( f(\mathbf{x}) = f(x_1, y_1, x_2, y_2, x_3, y_3) \) is relatively easy to compute for any particular state once we compute the closest cities. Let \( C_i \) be the set of cities whose closest airport.

### Discretization

One way to deal with a continuous state space is to discretize it. For example, instead of allowing the \( (x_i, y_i) \) locations to be any point in continuous two-dimensional space, we could limit them to fixed points on a rectangular grid with spacing of size \( \delta \) (delta). Then instead of having an infinite number of successors, each state in the space would have only 12 successors, corresponding to incrementing one of the 6 variables by \( \pm\delta \). We can then apply any of our local search algorithms to this discrete space. Alternatively, we could make the branching factor finite by sampling successor states randomly, moving in a random direction by a small amount, \( \delta \). Methods that measure progress by the change in the value of the objective function between two nearby points are called empirical gradient methods. Empirical gradient search is the same as steepest-ascent hill climbing in a discretized version of the state space. Reducing the value of \( \delta \) over time can give a more accurate solution, but does not necessarily converge to a global optimum in the limit.

### Gradient

Often we have an objective function expressed in a mathematical form such that we can use calculus to solve the problem analytically rather than empirically. Many methods attempt to use the gradient of the landscape to find a maximum. The gradient of the objective function is a vector \( \nabla f \) that gives the magnitude and direction of the steepest slope.

In some cases, we can find a maximum by solving the equation. In many cases, however, this equation cannot be solved in closed form. For example, with three airports, the expression for the gradient depends on what cities are closest to each airport in the current state. This means we can compute the gradient locally (but not globally).


## 4.3 Search with Nondeterministic Actions

In Chapter 3, we assumed a fully observable, deterministic, known environment. Therefore, an agent can observe the initial state, calculate a sequence of actions that reach the goal, and execute the actions with its "eyes closed," never having to use its percepts.

When the environment is partially observable, however, the agent doesn't know for sure what state it is in; and when the environment is nondeterministic, the agent doesn't know what state it transitions to after taking an action. That means that rather than thinking "I'm in state s1 and if I do action a I'll end up in state s2," an agent will now be thinking "I'm either in state s1 or s3, and if I do action a I'll end up in state s2, s4 or s5." We call a set of physical states that the agent believes are possible a *belief state*.

In partially observable and nondeterministic environments, the solution to a problem is no longer a sequence, but rather a *conditional plan* (sometimes called a contingency plan or a strategy) that specifies what to do depending on what percepts agent receives while executing the plan. We examine nondeterminism in this section and partial observability in the next.

### 4.3.1 The erratic vacuum world

The vacuum world from Chapter 2 has eight states, as shown in Figure 4.9. There are three actions—Right, Left, and Suck—and the goal is to clean up all the dirt (states 7 and 8). If the environment is fully observable, deterministic, and completely known, then the problem is easy to solve with any of the algorithms in Chapter 3, and the solution is an action sequence.

Now suppose that we introduce nondeterminism in the form of a powerful but erratic vacuum cleaner. In the erratic vacuum world, the Suck action works as follows:

- When applied to a dirty square the action cleans the square and sometimes cleans up dirt in an adjacent square, too.
- When applied to a clean square the action sometimes deposits dirt on the carpet.

To provide a precise formulation of this problem, we need to generalize the notion of a transition model from Chapter 3. Instead of defining the transition model by a RESULT function that returns a single outcome state, we use a RESULTS function that returns a set of possible outcome states.

### AND–OR search trees

How do we find these contingent solutions to nondeterministic problems? As in Chapter 3, we begin by constructing search trees, but here the trees have a different character. In a deterministic environment, the only branching is introduced by the agent’s own choices in each state: I can do this action or that action. We call these nodes *OR nodes*. In the vacuum world, for example, at an OR node the agent chooses Left or Right or Suck. In a nondeterministic environment, branching is also introduced by the environment’s choice of outcome for each action. We call these nodes *AND nodes*.

```python
function AND-OR-SEARCH(problem) returns a conditional plan, or failure
    return OR-SEARCH(problem, problem.INITIAL, [])

function OR-SEARCH(problem,state, path) returns a conditional plan, or failure
    if problem.IS-GOAL(state) then return the empty plan
    if IS-CYCLE(state, path) then return failure
    for each action in problem.ACTIONS(state) do
        plan←AND-SEARCH(problem, RESULTS(state, action), [state] + [path])
        if plan ≠ failure then return [action] + [plan]
    return failure

function AND-SEARCH(problem,states, path) returns a conditional plan, or failure
    for each si in states do
        plani←OR-SEARCH(problem,si, path)
        if plani = failure then return failure
    return [if s1 then plan1 else if s2 then plan2 else ...if sn−1 then plann−1 else plann]
```

## 4.4 Search in Partially Observable Environments

We now turn to the problem of partial observability, where the agent's percepts are not enough to pin down the exact state. That means that some of the agent's actions will be aimed at reducing uncertainty about the current state.

###  Searching with no observation

When the agent's percepts provide no information at all, we have what is called a *sensorless* or *conformant problem*. At first, you might think the sensorless agent has no hope of solving a problem if it has no idea what state it starts in, but sensorless solutions are surprisingly common and useful, primarily because they don't rely on sensors working properly.

Consider a sensorless version of the (deterministic) vacuum world. Assume that the agent knows the geography of its world, but not its own location or the distribution of dirt. In that case, its initial belief state is `{1,2,3,4,5,6,7,8}`. Now, if the agent moves Right, it will be in one of the states `{2,4,6,8}`—the agent has gained information without perceiving anything! After `[Right, Suck]` the agent will always end up in one of the states `{4,8}`. Finally, after `[Right, Suck, Left, Suck]` the agent is guaranteed to reach the goal state 7, no matter what the start state. We say that the agent can *coerce* the world into state 7.

The solution to a sensorless problem is a sequence of actions, not a conditional plan (because there is no perceiving). But we search in the space of belief states rather than physical states.

###  Searching in partially observable environments

Many problems cannot be solved without sensing. For a partially observable problem, the problem specification will specify a `PERCEPT(s)` function that returns the percept received by the agent in a given state. Consider a local-sensing vacuum world, in which the agent has a position sensor that yields the percept `L` in the left square, and `R` in the right square, and a dirt sensor that yields `Dirty` when the current square is dirty and `Clean` when it is clean. Thus, the `PERCEPT` in state 1 is `[L, Dirty]`.

###  Solving partially observable problems

The preceding section showed how to derive the `RESULTS` function for a nondeterministic belief-state problem from an underlying physical problem, given the `PERCEPT` function. With this formulation, the AND–OR search algorithm can be applied directly to derive a solution. The solution is the conditional plan `[Suck, Right, if Rstate={6} then Suck else []]`.

### An agent for partially observable environments

An agent for partially observable environments formulates a problem, calls a search algorithm (such as AND-OR-SEARCH) to solve it, and executes the solution. The solution will be a conditional plan rather than a sequence; to execute an if–then–else expression, the agent will need to test the condition and execute the appropriate branch of the conditional.

Given an initial belief state `b`, an action `a`, and a percept `o`, the new belief state is: b' = UPDATE(PREDICT(b,a),o).
This function goes under various names, including monitoring, filtering, and state estimation. It's called a recursive state estimator because it computes the new belief state from the previous one rather than by examining the entire percept sequence.

## 4.5 Online Search Agents and Unknown Environments

So far we have concentrated on agents that use offline search algorithms. They compute a complete solution before taking their first action. In contrast, an online search agent interleaves computation and action: first it takes an action, then it observes the environment and computes the next action. Online search is also helpful in nondeterministic domains because it allows the agent to focus its computational efforts on the contingencies that actually arise rather than those that might happen but probably won’t.

###  Online search problems

An online search problem is solved by interleaving computation, sensing, and acting. We’ll start by assuming a deterministic and fully observable environment and stipulate that the agent knows only the following:

- `ACTIONS(s)`, the legal actions in state `s`;
- `c(s,a,s')`, the cost of applying action `a` in state `s` to arrive at state `s'`;
- `IS-GOAL(s)`, the goal test.

The agent cannot determine `RESULT(s, a)` except by actually being in `s` and doing `a`.

###  Online search agents

After each action, an online agent in an observable environment receives a percept telling it what state it has reached; from this information, it can augment its map of the environment. The updated map is then used to plan where to go next.

```python
function ONLINE-DFS-AGENT(problem, s0) returns an action
    s, a, the previous state and action, initially null
    result, a table mapping (s, a) to s0, initially empty
    untried, a table mapping s to a list of untried actions
    unbacktracked, a table mapping s to a list of states never backtracked to
    if problem.IS-GOAL(s0) then return stop
    if s0 is a new state (not in untried) then untried[s0]←problem.ACTIONS(s0)
    if s is not null then
        result[s, a]←s0
        add s to the front of unbacktracked[s0]
    if untried[s0] is empty then
        if unbacktracked[s0] is empty then return stop
        a←an action b such that result[s0, b] = POP(unbacktracked[s0])s0← null
    else a←POP(untried[s0])
    s←s0
    return a
```

### Online local search
Like depth-first search, hill-climbing search has the property of locality in its node expansions. However, the basic algorithm is not very good for exploration because it leaves the agent sitting at local maxima with nowhere to go.

An agent implementing the LRTA* scheme is called learning real-time A* (LRTA*), which selects an action according to the values of neighboring states, which are updated as the agent moves about the state space.

### Learning in online search
The initial ignorance of online search agents provides several opportunities for learning. First, the agents learn a “map” of the environment—more precisely, the outcome of each action in each state—simply by recording each of their experiences.

## Summary 


This chapter has examined search algorithms for problems in partially observable, nondeterministic, unknown, and continuous environments.
- _Local search_ methods such as **hill climbing** keep only a small number of states in
memory. They have been applied to optimization problems, where the idea is to find a
high-scoring state, without worrying about the path to the state. Several stochastic local
search algorithms have been developed, including **simulated annealing**, which returns
optimal solutions when given an appropriate cooling schedule.
- Many local search methods apply also to problems in continuous spaces. **Linear programming** and **convex optimization** problems obey certain restrictions on the shape
of the state space and the nature of the objective function, and admit polynomial-time
algorithms that are often extremely efficient in practice. For some mathematically wellformed problems, we can find the maximum using calculus to find where the gradient
is zero; for other problems we have to make do with the empirical gradient, which
measures the difference in fitness between two nearby points.
- An **evolutionary algorithm** is a stochastic hill-climbing search in which a population
of states is maintained. New states are generated by **mutation** and by **crossover**, which
combines pairs of states.
- In **nondeterministic** environments, agents can apply AND–OR search to generate **contingent** plans that reach the goal regardless of which outcomes occur during execution.
- When the environment is partially observable, the **belief state** represents the set of
possible states that the agent might be in.
- Standard search algorithms can be applied directly to belief-state space to solve **sensorless problems**, and belief-state AND–OR search can solve general partially observable
problems. Incremental algorithms that construct solutions state by state within a belief
state are often more efficient.
- **Exploration problems** arise when the agent has no idea about the states and actions of
its environment. For safely explorable environments, online search agents can build a
map and find a goal if one exists. Updating heuristic estimates from experience provides
an effective method to escape from local minima.
