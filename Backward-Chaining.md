# Backward Chaining in First-Order Logic

## Summary

We have presented an analysis of logical inference in first-order logic and a number of algorithms for doing it.

- A first approach uses inference rules (universal instantiation and existential instantiation) to propositionalize the inference problem. Typically, this approach is slow unless the domain is small.
- The use of unification to identify appropriate substitutions for variables eliminates the instantiation step in first-order proofs, making the process more efficient in many cases.
- A lifted version of Modus Ponens uses unification to provide a natural and powerful inference rule, generalized Modus Ponens. The forward-chaining and backward-chaining algorithms apply this rule to sets of definite clauses.
- Generalized Modus Ponens is complete for definite clauses, although the entailment problem is semidecidable. For Datalog knowledge bases consisting of function-free definite clauses, entailment is decidable.
- Forward chaining is used in deductive databases, where it can be combined with relational database operations. It is also used in production systems, which perform efficient updates with very large rule sets. Forward chaining is complete for Datalog and runs in polynomial time.
- Backward chaining is used in logic programming systems, which employ sophisticated compiler technology to provide very fast inference. Backward chaining suffers from redundant inferences and infinite loops; these can be alleviated by memoization.
- Prolog, unlike first-order logic, uses a closed world with the unique names assumption and negation as failure. These make Prolog a more practical programming language, but bring it further from pure logic.
- The generalized resolution inference rule provides a complete proof system for first-order logic, using knowledge bases in conjunctive normal form.
- Several strategies exist for reducing the search space of a resolution system without compromising completeness. One of the most important issues is dealing with equality; we showed how demodulation and paramodulation can be used.
- Efficient resolution-based theorem provers have been used to prove interesting mathematical theorems and to verify and synthesize software and hardware.

## Backward Chaining in First-Order Logic

###  Backward Chaining

#### 9.4.1 A backward-chaining algorithm

Backward chaining works backward from the goal, chaining through rules to find known facts supporting the proof.

`FOL-BC-ASK(KB, goal)`: proved if KB contains a rule of the form lhs ⇒ goal.

`FOL-BC-ASK` is implemented as a generator, providing multiple possible results.

####  Logic programming

Logic programming embodies the declarative ideal, constructing systems by expressing knowledge in a formal language.

Prolog is a widely used logic programming language known for rapid prototyping and symbol manipulation tasks.

Prolog programs consist of sets of definite clauses and use depth-first backward chaining. They also incorporate some built-in functions and predicates for efficiency.

####  Redundant inference and infinite loops

Prolog's mismatch between depth-first search and search trees can lead to infinite loops.

Depth-first backward chaining may encounter problems with redundant computations.

Tabled logic programming combines backward chaining with dynamic-programming efficiency.

#### Database semantics of Prolog

Prolog uses database semantics with unique names and closed world assumptions.

Efficiency and conciseness in Prolog come from these assumptions.

Comparison with first-order logic in expressing knowledge about the number of objects.

####  Constraint logic programming

Constraint logic programming (CLP) allows variables to be constrained, not just bound.

CLP systems incorporate various constraint-solving algorithms and offer a more flexible approach to solving logic programming queries.

## Conclusion

Backward chaining in Prolog is popular but has limitations. Logic programming, especially Prolog, is widely used for various applications. Constraint logic programming extends the capabilities of standard logic programming.
