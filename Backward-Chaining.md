
## 9.4 Backward Chaining

### 9.4.1 A backward-chaining algorithm
- Backward chaining: works backward from the goal, chaining through rules to find known facts supporting the proof.
- FOL-BC-ASK(KB, goal): proved if KB contains a rule of the form lhs ⇒ goal.
- FOL-BC-ASK is implemented as a generator, providing multiple possible results.

### 9.4.2 Logic programming
- Logic programming: embodies the declarative ideal, constructing systems by expressing knowledge in a formal language.
- Prolog: widely used logic programming language; rapid prototyping, symbol manipulation tasks.
- Prolog programs: sets of definite clauses; uses depth-first backward chaining; some built-in functions and predicates for efficiency.

### 9.4.3 Redundant inference and infinite loops
- Prolog's mismatch between depth-first search and search trees can lead to infinite loops.
- Depth-first backward chaining: problems with redundant computations.
- Tabled logic programming: combines backward chaining with dynamic-programming efficiency.

### 9.4.4 Database semantics of Prolog
- Prolog uses database semantics with unique names and closed world assumptions.
- Prolog's efficiency and conciseness come from these assumptions.
- Comparison with first-order logic in expressing knowledge about the number of objects.

### 9.4.5 Constraint logic programming
- Constraint logic programming (CLP): allows variables to be constrained, not just bound.
- CLP systems: incorporate various constraint-solving algorithms; more flexible approach to solving logic programming queries.

## Conclusion
- Backward chaining in Prolog is popular but has limitations.
- Logic programming, especially Prolog, widely used for various applications.
- Constraint logic programming extends the capabilities of standard logic programming.
