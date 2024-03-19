# Forward Chaining in First-Order Logic

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

## Forward Chaining in First-Order Logic

###  Forward Chaining

In Section 7.5, a forward-chaining algorithm for propositional definite clauses was introduced. Here, the concept is extended to cover first-order definite clauses. Definite clauses in the form Antecedent ⇒ Consequent are used to represent real-world systems.

####  First-order definite clauses

First-order definite clauses are disjunctions of literals, where exactly one is positive. They can be atomic or implication statements with a conjunction in the antecedent and a single positive literal in the consequent. Quantifiers are implicit, and a typical first-order definite clause looks like King(x)∧Greedy(x) ⇒ Evil(x).

####  Representing a problem using definite clauses

A problem involving an American selling weapons to a hostile nation is represented using first-order definite clauses. Rules and facts are formulated, such as the crime-related rules American(x)∧Weapon(y)∧Sells(x, y, z)∧Hostile(z) ⇒ Criminal(x).

####  Forward-chaining algorithm

A simple forward-chaining algorithm is introduced. Starting from known facts, it triggers rules whose premises are satisfied, adding conclusions to known facts. This process repeats until the query is answered or no new facts are added.

#### Efficient Forward Chaining

- **Matching rules against known facts**: The inner loop of the algorithm involves matching rules against facts, with challenges in matching complex rules efficiently. Despite some NP-hardness in matching, most real-world rules are small, making data complexity polynomial.

- **Incremental forward chaining**: An incremental forward-chaining algorithm is introduced, where redundant rule matching is avoided by considering only rules relevant to new facts inferred in the previous iteration. This improves efficiency.

- **Irrelevant facts**: Efficiency is further addressed by considering the relevance of inferences. Techniques such as the Rete algorithm and deductive databases are introduced to optimize forward chaining, ensuring that only relevant conclusions are drawn.
