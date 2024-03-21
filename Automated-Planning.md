# Automated Planning
In this chapter, we explore how an agent can efficiently construct complex plans of action by taking advantage of the problem's structure. Planning is essential for intelligent agents to navigate discrete, deterministic, static, and fully observable environments. The chapter delves into a general factored representation language for planning problems—PDDL (Planning Domain Definition Language)—which offers a natural, succinct, scalable, and heuristic-free domain representation. We discuss efficient planning algorithms, heuristics, handling of non-classical domains, and scheduling problems with resource constraints, moving towards real-world applications in spacecraft, factories, and military operations planning.

## 11.1 Definition of Classical Planning

Classical planning involves finding a sequence of actions to achieve a goal within a discrete, deterministic, static, and fully observable environment. Key points include:

- **Factored Representation**: Utilizing PDDL, planning problems can represent a wide variety of domains efficiently and without the need for domain-specific heuristics.
  
- **States and Actions**: States are represented as conjunctions of ground atomic fluents. Action schemas represent families of actions, defined by their precondition and effect.
  
- **Planning Domain and Problems**: A set of action schemas defines a planning domain. Specific problems are defined by an initial state and a goal.

### Example Domains

- **Air Cargo Transport**: Involves actions like loading, unloading cargo, and flying. It's defined with actions affecting the predicates `In(c, p)` and `At(x, a)`.

- **The Spare Tire Problem**: Simplifies the challenge of changing a flat tire into abstract actions like removing the spare tire from the trunk and mounting it onto the car's axle.

- **The Blocks World**: A domain involving stacking cube-shaped blocks. Actions include moving a block from one position to another, constrained by the blocks' arrangement and a robot arm's capabilities.

## Planning with PDDL

PDDL allows for expressive and efficient problem definitions, including:

- **Initial State and Goal**: Defined as a conjunction of ground fluents for the initial state and a conjunction of literals for the goal.

- **Action Schemas**: Define possible actions within the domain, their preconditions, and their effects on the state.

- **Solving Planning Problems**: Solutions are sequences of actions that transition from the initial state to a state satisfying the goal conditions.

## Key Concepts

- **Applicability and Result of Actions**: An action is applicable if its precondition is satisfied by the current state. The result is a new state reflecting the action's effects.

- **Complexity and Scalability**: Classical planning faces challenges in complexity and scalability, particularly as domain size increases.

- **Extensions for Non-classical Domains**: PDDL has been extended to handle continuous, partially observable, concurrent, and multi-agent domains.

- **Hierarchical Actions and Scheduling**: Section 11.4 introduces hierarchical actions for tackling more complex problems, and Section 11.6 covers scheduling with resource constraints.

## Summary

This chapter establishes a foundational understanding of automated planning in AI. By leveraging structured representations and efficient algorithms, planning systems can devise action sequences to achieve goals across various domains. The development of PDDL and its extensions play a crucial role in addressing the inherent complexity of planning problems, offering a path towards more sophisticated and practical planning applications in real-world scenarios.


# 11.2 Algorithms for Classical Planning

Planning problems can be approached by searching from the initial state towards the goal, searching backward from the goal towards the initial state, or translating the problem into a set of logical sentences for logical inference.

## Forward state-space search for planning

Applying heuristic search algorithms from earlier chapters, we explore ground states where every fluent is definitively true or false. The goal state has all positive and none of the negative fluents defined in the problem's goal. Actions(s) are the grounded instantiations of action schemas that apply to a given state.

The size of the state space can seem prohibitive, but domain-independent heuristics (discussed in Section 11.3) make forward search feasible by deriving strong heuristics automatically.

## Backward search for planning

Also known as regression search, this approach begins at the goal, applying actions backward. It considers *relevant actions*, reducing the branching factor and focusing on actions that directly contribute to achieving the goal state. Regression search uses state descriptions with variables, which complicates heuristic development but often results in lower branching factors than forward search.

##  Planning as Boolean satisfiability

SAT-based planners like SATPLAN translate PDDL problems into propositional logic, leveraging the efficiency of modern SAT solvers. This involves propositionalizing actions, adding action exclusion and precondition axioms, defining the initial state and goal propositionally, and adding successor-state axioms. Although this translation results in a larger propositional set, the speed of SAT solvers often compensates for the increased size.

## Other classical planning approaches

- **Graphplan**: Uses a planning graph to encode constraints and relationships between actions, preconditions, and effects, addressing mutual exclusivity.
- **Situation calculus**: Describes planning problems in first-order logic, focusing on successor-state axioms. It has contributed theoretically but is less practical due to the developmental state of first-order provers.
- **Constraint Satisfaction Problem (CSP)** encoding: Simplifies the encoding to SAT by using single variables for actions at each time step, avoiding the need for action exclusion axioms.
- **Partial-order planning**: Represents plans as graphs rather than sequences, allowing for more flexibility and addressing independent subproblems. Although not competitive in fully automated classical planning, partial-order planning is preferred for operations scheduling and domains requiring human-readable plans.

Overall, the choice of planning approach depends on the specific requirements and characteristics of the planning problem, including the need for domain-specific heuristics, the complexity of the state space, and the computational resources available.

# 11.3 Heuristics for Planning

Efficient search in planning requires good heuristics. Factored representations of states and actions allow for the definition of domain-independent heuristics.

## Defining Admissible Heuristics through Relaxed Problems

An admissible heuristic can be derived from a relaxed problem, which is simpler and can estimate the distance to the goal. Relaxation can involve adding more edges to the graph or grouping nodes together for abstraction.

### Ignore-Preconditions Heuristic

This heuristic simplifies actions by dropping all preconditions, making every action applicable in every state. The heuristic is almost the number of unsatisfied goals, but adjustments may be needed due to actions achieving multiple goals or undoing effects of others.

### Ignore-Delete-Lists Heuristic

By removing the delete lists from actions, we ensure that no action can undo the progress made by another. This creates a relaxed problem where monotonic progress towards the goal is possible, facilitating a polynomial-time solution via hill climbing, despite the optimal solution being NP-hard.

![Two state spaces from planning problems with the ignore-delete-lists heuristic.](https://example.com/ignore-delete-lists-heuristic.png)

*Figure 11.6: State spaces with ignore-delete-lists heuristic show no local minima, simplifying the search for the goal.*

## Domain-Independent Pruning

Factored representations expose that many states are merely variants of others. Techniques like symmetry reduction and forward pruning help in focusing the search on promising branches without exploring all permutations of actions.

### Preferred Actions

A preferred action is identified by solving a relaxed version of the problem and then choosing actions that are steps of the relaxed plan or achieve its preconditions, focusing the search on relevant actions.

### Serializable Subgoals

Problems with serializable subgoals allow for subgoals to be achieved in a certain order without undoing previous achievements, significantly reducing the complexity of the problem.

## State Abstraction in Planning

State abstraction reduces the number of states by mapping multiple ground states to an abstract representation. This can involve ignoring certain fluents or decomposing the problem into parts to be solved independently.

### Decomposition and Subgoal Independence

Decomposing a problem into subgoals and solving each independently can be an effective strategy, especially when subgoals are independent. Combining cost estimates from subplans can provide a more accurate or admissible estimate than considering each subgoal in isolation.

FF (FASTFORWARD) uses the ignore-delete-lists heuristic alongside a planning graph to estimate costs and employs a modified hill-climbing search to find solutions. Its approach to handling local maxima and switching to greedy best-first search when necessary has shown effectiveness in finding solutions to complex planning problems.


# 11.4 Hierarchical Planning

Hierarchical planning addresses the scalability issue of planning by introducing higher levels of abstraction. It allows for the planning of complex activities that would otherwise require an impractical number of atomic actions.

## High-level Actions and Hierarchical Task Networks (HTN)

High-level actions (HLAs) are actions that can be refined into sequences of simpler actions, which may themselves be HLAs or primitive actions. Hierarchical task networks (HTN) planning utilizes these HLAs to structure planning into manageable levels of detail.

### Implementations and Refinements

- **Implementation:** A sequence of primitive actions that execute an HLA.
- **Refinement:** The process of breaking down an HLA into a sequence of simpler actions.

A high-level plan achieves the goal if at least one of its implementations does.

## Searching for Primitive Solutions

HTN planning can be approached by searching for a sequence of refinements that lead from a top-level action to a plan achieving the goal. The search progresses by iteratively refining HLAs until a plan consisting solely of primitive actions is formed.

## Searching for Abstract Solutions

An alternative approach involves defining preconditions and effects for HLAs, allowing for the planning at an abstract level without immediately considering their specific implementations. This method aims for the "downward refinement property," ensuring that an abstract plan can be successfully refined into a concrete plan achieving the goal.

### Angelic Semantics and Reachable Sets

Under angelic semantics, the effects of an HLA are defined in terms of the "reachable set" of states that the HLA can lead to, considering the choice of implementations. This approach allows for more flexible planning, acknowledging that different implementations of an HLA can achieve different outcomes.

## Benefits of Hierarchical Planning

- **Complexity Management:** Hierarchical decomposition simplifies complex planning problems by breaking them down into more manageable sub-tasks.
- **Knowledge Reuse:** HLAs encapsulate knowledge about how to achieve complex goals, enabling the reuse of planning strategies across different problems.
- **Human-like Planning:** The process of planning at higher levels of abstraction and refining those plans into concrete actions mirrors human problem-solving strategies.

Hierarchical planning, especially with angelic semantics, offers a powerful method for solving complex planning problems efficiently, drawing on the inherent structure and decomposability of many real-world tasks.

# 11.5 Planning and Acting in Nondeterministic Domains

This section explores extending planning to environments that are partially observable, nondeterministic, or unknown. It builds on concepts from Chapter 4 but adapts them to the factored representation of planning problems, affecting action and observation representation and belief state management.

## Key Concepts

- **Sensorless (Conformant) Planning:** Deals with environments where no observations are available. The agent must reason about actions that lead to the goal without observational feedback.

- **Contingency Planning:** For environments that are partially observable and nondeterministic. Plans include conditional branches based on potential observations, allowing the agent to adapt to different outcomes.

- **Online Planning and Replanning:** For unknown environments. The agent plans, acts, and then replans based on the outcomes of actions and new observations, addressing inaccuracies in the model or unexpected changes.

### Example Problem

Given a chair and table, the goal is for them to match in color. The initial state includes two cans of paint with unknown colors. Actions include removing the lid from a paint can and painting an object with an open can. The agent might not initially know the colors of the paint or furniture.

## Sensorless Planning

For sensorless problems, planning occurs in a belief-state space. The initial belief state represents what the agent knows and doesn't know about the environment. The agent searches for action sequences that transition from the initial belief state to a belief state where the goal is satisfied, managing belief states as logical formulas.

## Contingent Planning

Contingent planning generates plans with branches based on expected percepts, suitable for partially observable and nondeterministic environments. The agent uses its sensor model, represented as percept schemas in PDDL, to reason about future observations and make decisions. Plans may include conditions like "if the table and chair are the same color, then no action is needed; otherwise, find a matching paint can."

## Online Planning

Online planning involves planning, executing actions, and then replanning based on new information. This approach is flexible and can adapt to changes in the environment, inaccuracies in the agent's world model, or unexpected events. Online planning might involve execution monitoring techniques such as:

- **Action Monitoring:** Verifying preconditions of the next action before execution.
- **Plan Monitoring:** Checking if the remainder of the plan is still viable given the current state.
- **Goal Monitoring:** Considering whether there are better goals to pursue based on new information.

### Execution Monitoring and Replanning

Execution monitoring identifies when plans need to be adjusted or replaced. This might be due to realizing a plan is unfeasible, encountering unexpected outcomes, or achieving goals earlier than expected. Replanning involves generating a new plan from the current state to the goal, possibly revising the agent's understanding of the world.

## Challenges and Solutions

- **Dealing with Incomplete or Incorrect Models:** When an agent's model of the world is incomplete or incorrect, it may need to learn from its experiences and update its model to improve future planning and execution.

- **Avoiding Dead Ends:** Ensuring that the agent can always find a way to progress towards the goal, despite nondeterminism or partial observability, by selecting among multiple potential repair strategies or learning from failures.

Online planning and execution monitoring allow an agent to act intelligently in complex and changing environments, embodying a level of adaptiveness and purpose that is crucial for real-world applications.


# 11.6 Time, Schedules, and Resources

Classical planning primarily focuses on the sequence of actions but does not account for their durations or scheduling. This section introduces methods to incorporate time constraints, scheduling, and resource management into planning.

## Introduction to Scheduling and Resources

In real-world scenarios, planning must consider not just the sequence of actions but also their timing and resource constraints. For instance, airlines have to manage limited resources like staff and aircraft, ensuring that schedules are feasible and efficient.

### Example: Job-Shop Scheduling Problem

A job-shop scheduling problem involves a set of jobs, each comprising multiple actions with specific orderings, durations, and resource requirements. The goal is to schedule these actions while satisfying temporal and resource constraints to minimize the total duration, known as the *makespan*.

**Figure 11.13 Example:**

```plaintext
Jobs({AddEngine1 ≺ AddWheels1 ≺ Inspect1}, {AddEngine2 ≺ AddWheels2 ≺ Inspect2})
Resources(EngineHoists(1), WheelStations(1), Inspectors(2), LugNuts(500))
Action(AddEngine1, DURATION:30, USE:EngineHoists(1))
Action(AddEngine2, DURATION:60, USE:EngineHoists(1))
Action(AddWheels1, DURATION:30, CONSUME:LugNuts(20), USE:WheelStations(1))
Action(AddWheels2, DURATION:15, CONSUME:LugNuts(20), USE:WheelStations(1))
Action(Inspecti, DURATION:10, USE:Inspectors(1))
```
This example outlines a job-shop scheduling problem for assembling two cars, detailing actions, their durations, and resource constraints.

## Solving Scheduling Problems
### Temporal Scheduling
Initially, we focus on temporal aspects, determining the earliest and latest start times for actions based on ordering constraints. This can be achieved using the Critical Path Method (CPM), identifying the sequence of actions that determines the total duration of the plan.

### Resource Constraints
Incorporating resource constraints adds complexity, as actions requiring the same resource cannot overlap. The scheduling problem becomes NP-hard when accounting for resources. Various heuristic and optimization techniques are employed to find feasible schedules that minimize the makespan.

### Planning and Scheduling Integration
Rather than treating planning and scheduling as separate phases, an integrated approach considers action durations and resource constraints during plan formation. This can lead to more efficient and feasible plans, as the planning algorithm accounts for temporal and resource-related challenges from the outset.

### Approaches and Challenges
Solving scheduling problems with resource constraints is challenging but essential for applications in manufacturing, logistics, and other fields. Techniques like the minimum slack heuristic, branch-and-bound, and simulated annealing are among the methods used to tackle these problems, aiming to find efficient schedules that meet all specified constraints.

## 11.7 Analysis of Planning Approaches

Planning in AI synergizes search and logic, navigating between generating solutions and proving their existence. This fusion has propelled planners from handling simple scenarios with a dozen states and actions to tackling real-world applications involving millions of states and thousands of actions.

The core challenge in planning is managing the combinatorial explosion inherent in the domain. For a domain with \(n\) propositions, there are \(2^n\) potential states. While this outlook may seem grim, identifying independent subproblems within the planning task can significantly mitigate the issue. In cases of full decomposability, the speedup can be exponential, dramatically enhancing efficiency. However, this advantage is compromised by negative interactions between actions, which can reintroduce complexity.

SATPLAN showcases one approach to managing the complexity by encoding logical relationships between subproblems, allowing for intelligent navigation of the problem space even when subproblems are not fully independent. Forward search strategies aim to identify patterns or subsets of propositions that hint at these independent subproblems, applying heuristic methods to progress even without complete problem independence.

The field has yet to converge on a definitive understanding of which planning techniques excel for specific problem types. Future developments may integrate the expressiveness of first-order and hierarchical representations with the efficiency of current factored and propositional methods. The concept of portfolio planning systems represents an emerging direction, where multiple algorithms are available to tackle any given problem. These systems may employ selective application (matching problems with the most suitable algorithm), parallel execution (running all algorithms on separate CPUs), or a scheduled interleaving of techniques, all aimed at optimizing planning efficiency and effectiveness.

# Summary

In this chapter, we delved into the Planning Domain Definition Language (PDDL) for classical and extended planning problems and explored various algorithmic strategies for finding solutions. Key takeaways include:

- **Planning systems** operate on explicit, factored representations of states and actions, enabling the creation of domain-independent heuristics and the development of robust algorithms for problem-solving.
- **PDDL** outlines initial and goal states as conjunctions of literals, detailing actions through preconditions and effects. Extensions to PDDL accommodate time, resources, percepts, contingent plans, and hierarchical structures.
- **State-space search** strategies include forward direction (progression) and backward direction (regression), with subgoal independence assumptions and problem relaxations serving as effective heuristic bases.
- **Alternative approaches** encompass transforming planning problems into Boolean satisfiability problems or constraint satisfaction problems, and navigating the space of partially ordered plans.
- **Hierarchical Task Network (HTN) planning** utilizes high-level actions (HLAs) prescribed by domain designers, which can be implemented through various lower-level sequences. HLAs' effects can adopt angelic semantics, facilitating the derivation of correct high-level plans independent of their implementations. HTN is vital for generating the extensive plans required in real-world contexts.
- **Contingent planning** enables agents to sense and adapt during plan execution. Sensorless or conformant planning approaches can generate plans operational without direct perception. Constructing such plans involves searching within belief state spaces, where efficient belief state management is crucial.
- **Online planning** agents engage in execution monitoring and incorporate repairs as needed for recovery from unexpected outcomes due to nondeterministic actions, external events, or inaccuracies in environmental models.
- **Resource management** in planning often translates resources like time, materials, and finances into numeric values rather than managing each unit individually. Time, a critical resource, can be managed via specialized scheduling algorithms or integrated planning and scheduling.
- **Extending classical planning** to address nondeterministic environments opens the door to handling environments where actions' outcomes may be uncertain.

This chapter lays the groundwork for further exploration into planning under uncertainty, including stochastic environments in Chapter 16 (covering Markov decision processes and game theory) and reinforcement learning in Chapter 23, which focuses on behavior learning from past outcomes.

