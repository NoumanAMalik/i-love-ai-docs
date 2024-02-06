# Logical Agents - Chapter 7 Summary
Welcome to Chapter 7, where we explore the fascinating world of Logical Agents. In this chapter, we embark on a journey to design agents capable of understanding complex environments, using logical reasoning to derive new knowledge, and making informed decisions based on these insights.

## Understanding Knowledge-Based Agents
Humans leverage knowledge to navigate the world, and similarly, in the field of Artificial Intelligence, we design knowledge-based agents. These agents employ reasoning processes over an internal representation of knowledge to decide on the most appropriate actions.

## Limitations of Problem-Solving Agents
Previous chapters introduced problem-solving agents, but they have limitations. They lack general knowledge and rely on atomic representations, restricting their adaptability. For instance, a route-finding agent might struggle with abstract goals like driving to a town with a population less than 10,000.

## Factored Representations and Beyond
Chapter 5 introduced factored representations, a step towards domain-independent functioning. Chapter 7 takes this concept further, introducing logic as a general representation for knowledge-based agents. Logic empowers agents to combine and manipulate information for diverse purposes, from solving mathematical theorems to predicting Earth's life expectancy.

## Key Sections in Chapter 7
- 7.1 Overall Agent Design: The chapter begins with an overview of knowledge-based agent design.
- 7.2 Wumpus World: Introduces a simple environment, the "wumpus world," to illustrate knowledge-based agent operations.
- 7.3 General Principles of Logic: Explores the foundational principles of logic.
- 7.4 Propositional Logic: Details propositional logic as a factored representation.
- 7.5 and 7.6 Inference Technologies: Discusses well-developed inference technologies associated with logic.
- 7.7 Agents for Wumpus World: Combines knowledge-based agents with propositional logic to build practical agents for the wumpus world.

## Generic Knowledge-Based Agent
Here's a glimpse of a generic knowledge-based agent, which, given a percept, updates its knowledge base, queries for the best action, and records the taken action.

``` function KB-AGENT(percept) returns an action
  persistent: KB, a knowledge base
  t, a counter, initially 0, indicating time
  TELL(KB, MAKE-PERCEPT-SENTENCE(percept, t))
  action ← ASK(KB, MAKE-ACTION-QUERY(t))
  TELL(KB, MAKE-ACTION-SENTENCE(action, t))
  t ← t + 1
  return action
```

# 7.1 Knowledge-Based Agents

In this section, we explore the central component of a knowledge-based agent: the knowledge base (KB). A knowledge base is a set of sentences expressed in a knowledge representation language, forming assertions about the world. These sentences, also known as axioms, play a crucial role in decision-making.

## Key Concepts

### Knowledge Base (KB)
- The knowledge base is a collection of sentences representing the agent's understanding of the world.
  
### Sentence
- In this context, a sentence is a technical term related to, but not identical to, sentences in natural languages.

### Axiom
- A sentence that is given without being derived from other sentences, representing fundamental knowledge.

### TELL and ASK
- Operations to add new sentences (TELL) and query existing knowledge (ASK) in the knowledge base.
- Both operations may involve inference, ensuring that answers follow from previous information.

## Knowledge-Based Agent Program

![Knowledge-Based Agent Program](https://media.geeksforgeeks.org/wp-content/uploads/20210611205826/kba-300x271.png)

- The agent takes a percept as input, maintains a knowledge base (KB), and returns an action.
- Three main actions: TELLing what it perceives, ASKing for the next action, and TELLing the chosen action.
  
## Implementation Details

- Representation language details are encapsulated in functions like MAKE-PERCEPT-SENTENCE, MAKE-ACTION-QUERY, and MAKE-ACTION-SENTENCE.
- The agent, though similar to those with internal state, is distinctive due to the definitions of TELL and ASK.
  
## Knowledge Level vs. Implementation Level

- The knowledge-based agent can be described at the knowledge level, specifying what the agent knows and its goals, independent of implementation details.
- Example: An automated taxi's goal to take a passenger from San Francisco to Marin County, deciding to cross the Golden Gate Bridge based on knowledge.

## Declarative vs. Procedural Approach

- Declarative Approach: Building a knowledge-based agent by progressively TELLing it what it needs to know.
- Procedural Approach: Encoding desired behaviors directly as program code.
- Successful agents often combine both approaches for optimal design and efficiency.

## Learning Capabilities

- Mechanisms for a knowledge-based agent to learn for itself, creating general knowledge from a series of percepts.
- Learning agents can be fully autonomous.

# 7.2 The Wumpus World

In this section, we delve into the Wumpus World environment to showcase the capabilities of knowledge-based agents. The Wumpus World is a cave with rooms connected by passageways, featuring challenges such as the wumpus, pits, and the lure of gold.

## Environment Description

### Performance Measure
- +1000 for climbing out of the cave with gold
- -1000 for falling into a pit or being eaten by the wumpus
- -1 for each action taken
- -10 for using up the arrow
- Game ends on agent's death or successful climb out

### Environment
- A 4×4 grid of rooms with walls surrounding the grid
- Agent starts in [1,1], facing east
- Gold and wumpus locations are random, with uniform distribution
- Each non-start square has a 0.2 probability of being a pit

### Actuators
- Move Forward, TurnLeft, TurnRight, Grab, Shoot, Climb
- Agent dies if it enters a square with a pit or a live wumpus
- Bumping into a wall does not move the agent
- Grab picks up gold if in the same square
- Shoot fires an arrow in the direction the agent is facing, killing the wumpus or hitting a wall
- Only the first Shoot action has an effect
- Climb can be used to exit the cave, but only from square [1,1]

### Sensors
- Five sensors providing single-bit information each:
  - Stench: Wumpus nearby
  - Breeze: Pit nearby
  - Glitter: Gold in the same square
  - Bump: Agent bumped into a wall
  - Scream: Wumpus killed (perceived anywhere in the cave)

## Agent's Challenge

- Initial ignorance of the environment configuration
- Sequential and partially observable environment
- Agent's challenge involves logical reasoning to overcome its initial ignorance and make informed decisions

## Agent's Progress in the Wumpus World

### Example Scenario

1. Agent perceives [None,None,None,None,None], indicating safe squares [1,2] and [2,1].
2. Agent moves to [2,1] and perceives [None,Breeze,None,None,None], inferring a potential pit in [2,2] or [3,1].
3. Agent returns to [1,1] and moves to [1,2], perceiving [Stench,None,None,None,None], inferring the wumpus in [1,3].
4. Agent progresses, inferring safe squares and potential dangers until it reaches the gold.

## Logical Reasoning

- The agent uses logical reasoning to deduce information about the environment.
- Every conclusion drawn is correct if the available information is accurate.

In the upcoming sections, we explore how logical agents can represent information and draw conclusions to navigate complex environments like the Wumpus World.

# 7.3 Logic: Fundamental Concepts

This section provides a summary of the fundamental concepts of logical representation and reasoning.

## Logical Sentences and Syntax

- Knowledge bases consist of sentences expressed in a representation language.
- Sentences must adhere to the syntax of the representation language to be well-formed.
- Syntax is exemplified in ordinary arithmetic: e.g., "x+y = 4" is well-formed, "x4y+ =" is not.

## Semantics and Truth

- A logic defines the semantics or meaning of sentences.
- Truth of sentences is determined with respect to possible worlds.
- Possible models are mathematical abstractions with fixed truth values for each relevant sentence.
- The concept of satisfaction is introduced: a model satisfies a sentence if it is true in that model.

## Logical Reasoning: Entailment

- Logical reasoning involves the relation of logical entailment between sentences.
- Entailment is denoted as α |= β, meaning α entails β.
- Entailment is formalized: α |= β if and only if, in every model where α is true, β is also true.
- The direction of entailment (⊆) reflects the strength of the assertion: α is stronger than β.

## Logical Inference and Model Checking

- Inference algorithms are applied for logical reasoning.
- Model checking, an inference algorithm, enumerates possible models to check entailment.
- Entailment is illustrated in a wumpus-world reasoning example.
- Soundness: An inference algorithm is sound if it derives only entailed sentences.
- Completeness: Desirable property; an algorithm is complete if it can derive any entailed sentence.

## Correspondence to the Real World

- Logical reasoning corresponds to real-world relationships: derived conclusions are true if premises are true.
- The correspondence between the world and representation is illustrated.
- Grounding involves the connection between logical reasoning processes and the real environment.
- Sensors create the connection between syntax (KB) and the real world.

## Learning and Fallibility

- General rules in KB, derived from perceptual experience, are not identical to direct percept statements.
- Learning processes, discussed in Part V, contribute to constructing general rules.
- KB may not always be true in the real world, and fallibility exists.

In the next section, we explore Propositional Logic, a fundamental logic for knowledge representation.


# 7.4 Propositional Logic: A Very Simple Logic

In this section, we introduce propositional logic, covering its syntax, semantics, and a simple algorithm for logical inference within the context of the wumpus world.

## Syntax

### Atomic Sentences
- Proposition symbols represent true/false propositions.
- Symbol examples: P, Q, R, W1,3, FacingEast.
- Fixed symbols: True (always-true proposition) and False (always-false proposition).

### Complex Sentences
- Constructed from atomic sentences using logical connectives.
- Connectives: ¬ (not), ∧ (and), ∨ (or), ⇒ (implies), ⇔ (if and only if).
- Formal grammar provided in BNF notation with operator precedence.

## Semantics

- Models determine truth values for each proposition symbol.
- Recursive rules for computing truth values:
  - ¬P is true iff P is false.
  - P∧Q is true iff both P and Q are true.
  - P∨Q is true iff either P or Q is true.
  - P ⇒ Q is true unless P is true and Q is false.
  - P ⇔ Q is true iff P and Q are both true or both false.
- Truth tables for connectives guide truth value computations.

## Knowledge Base Construction

- Introduced symbols for wumpus world locations: Px,y, Wx,y, Bx,y, Sx,y, Lx,y.
- Constructed sentences (Ri) for immutable aspects of the wumpus world:
  - R1: ¬P1,1 (No pit in [1,1]).
  - R2: B1,1 ⇔ (P1,2 ∨ P2,1).
  - R3: B2,1 ⇔ (P1,1 ∨ P2,2 ∨ P3,1).
  - R4: ¬B1,1.
  - R5: B2,1.

## Inference Procedure

- Implemented a model-checking algorithm (TT-ENTAILS?) for logical inference.
- Enumerates models and checks if α is true in every model where KB is true.
- Time complexity: O(2^n), where n is the number of symbols.

In the subsequent sections, more advanced inference algorithms with improved efficiency are explored.

# 7.5 Propositional Theorem Proving

In this section, we explore propositional theorem proving as an alternative to model checking for determining entailment. The focus is on applying rules of inference directly to the knowledge base to construct proofs efficiently.

## Logical Equivalence

- **Logical Equivalence:** Two sentences α and β are equivalent (α ≡ β) if they are true in the same set of models.
- Equivalences, such as commutativity and distribution, are crucial for reasoning in logic.

## Validity and Satisfiability

- **Validity:** A sentence is valid if true in all models; also known as a tautology.
- **Satisfiability:** A sentence is satisfiable if true in some model.
- Connection: α is valid iff ¬α is unsatisfiable.

## Deduction Theorem

- For any sentences α and β, α |= β if and only if the sentence (α ⇒ β) is valid.
- The deduction theorem establishes a link between entailment and validity.

## Inference and Proofs

- **Inference Rules:**
  - **Modus Ponens:** α ⇒ β, α ⟹ β
  - **And-Elimination:** α∧β ⟹ α
  - Other logical equivalences can also be used as inference rules.

- **Proving ¬P1,2 (No pit in [1,2]):**
  1. Apply biconditional elimination to R2.
  2. Apply And-Elimination to the result.
  3. Use logical equivalence for contrapositives.
  4. Apply Modus Ponens with percept R4.
  5. Apply De Morgan's rule.
  6. Conclude ¬P1,2 ∧ ¬P2,1.

- **Proof Searching:**
  - Define a proof problem with initial knowledge base, actions (inference rules), result, and goal.
  - Search algorithms can find a sequence of steps constituting a proof.

- **Monotonicity:**
  - The set of entailed sentences increases with added information to the knowledge base.
  - KB |= α implies KB∧β |= α for any sentences α and β.

Propositional theorem proving offers efficiency advantages over model checking, especially in cases where the proof length is short compared to the number of models.

## 7.5.2 Proof by resolution

- Inference rules covered so far are sound but not necessarily complete.
- Introduction of the resolution rule, which, when coupled with a complete search algorithm, provides a complete inference algorithm.
- Resolution rule demonstrated in the wumpus world scenario.

### Unit Resolution Rule

- `1 ∨ ··· ∨ k, m` and `1 ∨ ··· ∨ i-1 ∨ i+1 ∨ ··· ∨ k` where each ` is a literal and `i` and `m` are complementary literals.
- Resolution rule can be generalized to the full resolution rule.
- Factoring, the removal of multiple copies of literals, is an aspect of the resolution rule.

### Conjunctive Normal Form (CNF)

- Resolution rule applies only to clauses (disjunctions of literals).
- Every sentence of propositional logic is logically equivalent to a conjunction of clauses in CNF.
  
## 7.5.3 Horn clauses and definite clauses

- The completeness of resolution makes it crucial.
- Some knowledge bases have restrictions, like definite clauses.
- Horn clauses, definite clauses, and goal clauses are introduced.
  
## 7.5.4 Forward and backward chaining

### Forward Chaining

- PL-FC-ENTAILS? algorithm for forward chaining is linear in time.
- It starts with known facts and propagates inferences until the query is found or no further inferences can be made.
- Sound and complete.

### Backward Chaining

- Works backward from the query.
- Efficient implementation running in linear time.
- Goal-directed reasoning, suitable for specific questions.


## 7.6 Effective Propositional Model Checking

In this section, two families of efficient algorithms for general propositional inference based on model checking are discussed. These algorithms focus on satisfiability (SAT) problems, specifically checking if a given propositional logic sentence is satisfiable.

### Overview

- Two families of algorithms: backtracking search and local hill-climbing search.
- Efficient algorithms for general propositional inference based on model checking.
- Important for handling complexity in various computer science problems.

### 7.6.1 A complete backtracking algorithm (DPLL)

#### Davis–Putnam–Logemann–Loveland (DPLL) Algorithm

- Based on a recursive, depth-first enumeration of possible models.
- Input: Sentence in conjunctive normal form (CNF).
- Improvements over TT-ENTAILS?:
  1. **Early Termination:** Detects truth or falsity with a partially completed model, avoiding unnecessary subtree exploration.
  2. **Pure Symbol Heuristic:** Identifies symbols that always appear with the same "sign" in all clauses.
  3. **Unit Clause Heuristic:** Focuses on clauses with only one unassigned literal.

#### Enhancements for scalability

1. **Component Analysis:** Detects disjoint subsets of clauses, improving solver speed.
2. **Variable and Value Ordering:** Considers variable frequency for better decision-making.
3. **Intelligent Backtracking:** Uses conflict clause learning to avoid repeated conflicts.
4. **Random Restarts:** Restarts the search tree when progress is not evident.
5. **Clever Indexing:** Fast indexing for optimization.

### 7.6.2 Local Search Algorithms

- Applies local search algorithms like HILL-CLIMBING and SIMULATED-ANNEALING to SAT problems.
- Evaluation function counts unsatisfied clauses.
- WALKSAT algorithm introduced, randomly flipping variable values.

### 7.6.3 The Landscape of Random SAT Problems

- SAT problems have varying difficulty.
- Easy: Few constraints relative to variables. 
- Hard: Close to the satisfiability threshold.
- **Threshold Conjecture:** Predicts a clause/symbol ratio where satisfiability probability sharply transitions.

#### Difficulty and Threshold

- Underconstrained problems are easy.
- Overconstrained problems are easier than those near the threshold.
- Hard problems often lie at the threshold.

**Note:** The provided pseudocode is a simplified representation without implementation details.

# 7.7 Agents Based on Propositional Logic

## Overview
This section discusses constructing wumpus world agents using propositional logic. The focus is on deducing the state of the world, logical inference, efficient world tracking, and constructing plans based on logical inference.

## Current State of the World
Logical agents operate by deducing actions from a knowledge base. The knowledge base consists of axioms and percept sentences. The agent's goal is to deduce the current state of the wumpus world, considering aspects like pits and wumpus locations.

- Axioms include statements about pits, wumpus, breezy and smelly squares.
- Propositions are associated with time steps, allowing the agent to handle changes over time.
- Successor-state axioms define how fluents change over time due to agent actions.
- The frame problem arises when determining what remains unchanged after an action.

## Hybrid Agent
A hybrid agent combines logical deduction with condition–action rules and problem-solving algorithms. The agent maintains a knowledge base and plans actions based on goals and current state deductions.

- The agent's program updates the knowledge base with percepts and time-dependent axioms.
- Logical inference helps determine safe squares and unvisited locations.
- The agent constructs plans based on goals, such as grabbing gold or exploring safe squares.
- A* search is used for route planning, and actions are prioritized based on goals.
- The agent may shoot at possible wumpus locations and adapt plans accordingly.
- If no safe squares are left, the agent attempts to create one by shooting or retreats if impossible.

## Challenges
- The frame problem involves specifying what remains unchanged after an action.
- The qualification problem arises when detailing exceptions for action failure.

# Logical State Estimation and Propositional Inference

## Logical State Estimation
The agent program presented in Figure 7.20 has a weakness where computational expense grows over time. To address this, the concept of logical state estimation is introduced. It involves caching the results of inference, allowing the process to build on earlier results rather than starting from scratch. The belief state, representing all possible current states of the world, is updated as new percepts arrive, a process known as state estimation.

### 1-CNF Belief State
Maintaining an exact belief state as a logical formula is challenging due to exponential growth in size. An approximate method involves representing belief states as conjunctions of literals (1-CNF formulas). Although this scheme may lose some information over time, it provides a conservative approximation to the exact belief state.

## Making Plans by Propositional Inference
The agent in Figure 7.20 uses A* search for planning but can also use logical inference for planning. The SATPLAN algorithm is introduced for propositional planning. It constructs a sentence with assertions about the initial state, successor-state axioms, and the goal. By presenting this sentence to a SAT solver, plans can be extracted.

### SATPLAN Algorithm
The SATPLAN algorithm tries different plan lengths, using a satisfiability solver to check if a plan is achievable. It constructs a sentence with information about the initial state, transitions, and goals. If the solver finds a model, a plan is extracted from the model. The algorithm repeats the process with an increased goal time step if no model is found.

### Challenges and Fixes
SATPLAN has challenges, such as generating plans with impossible actions or multiple simultaneous actions. To address these, precondition axioms and action exclusion axioms are introduced. These axioms ensure the validity of actions and prevent conflicts in simultaneous actions.

## Summary
- Logical state estimation involves maintaining a belief state using logical sentences.
- Propositional inference is used for planning through the SATPLAN algorithm.
- Challenges in SATPLAN, such as impossible actions and simultaneous actions, are addressed with precondition and action exclusion axioms.

**Note:** Propositional logic has limitations for handling unbounded environments, leading to the need for more expressive languages like first-order logic.

### Bibliographical and Historical Notes //Not Included 
