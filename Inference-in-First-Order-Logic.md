# Chapter 9: Inference in First-Order Logic

## Introduction
In Chapter 9, we delve into the realm of inference in first-order logic, unveiling effective procedures to answer questions posed within this logic. The chapter outlines algorithms capable of answering any answerable first-order logic question.

## Sections

### 9.1 Propositional vs. First-Order Inference
This section explores the conversion of first-order knowledge bases to propositional logic. It introduces inference rules for quantifiers and discusses Universal Instantiation and Existential Instantiation, enabling the reduction of first-order inference to propositional inference.

#### Universal Instantiation (UI)
- Allows the inference of sentences obtained by substituting ground terms for universally quantified variables.
- Formal representation: ∀v α → SUBST({v/g},α)

#### Existential Instantiation
- Replaces an existentially quantified variable with a new constant symbol.
- Formal representation: ∃v α → SUBST({v/k},α) where k is a new constant.

#### Reduction to Propositional Inference
- Demonstrates the conversion of any first-order knowledge base into a propositional knowledge base.
- Proposes the replacement of universally quantified sentences with all possible instantiations.

### 9.1.1 Reduction to Propositional Inference
This subsection expands on the conversion process, addressing the issue of infinite possibilities when function symbols are involved. Jacques Herbrand's theorem is introduced, stating that if a sentence is entailed, a proof involving a finite subset of the propositionalized knowledge base exists.

- **Propositionalization**
  - A general approach to first-order inference via propositionalization.
  - Achieves completeness but faces the challenge of determining nonentailment.

- **Semidecidability**
  - Discusses the semidecidability of entailment in first-order logic.
  - Acknowledges the existence of algorithms that affirm entailment but emphasizes the absence of an algorithm that uniformly denies nonentailment.


# 9.2 Unification and First-Order Inference

The propositionalization approach in first-order logic generates many unnecessary instantiations of universally quantified sentences. A more efficient approach is introduced through the Generalized Modus Ponens rule.

## Generalized Modus Ponens

The Generalized Modus Ponens rule allows conclusions to be asserted based on a single substitution that makes the conjuncts of the premise identical to sentences in the knowledge base. It is a lifted version of Modus Ponens, extending from propositional logic to first-order logic.

**Generalized Modus Ponens Rule:**

For atomic sentences pi, pi0, and q, with substitution θ such that:
SUBST(θ, pi0) = SUBST(θ, pi) for all i,
(p1 ∧ p2 ∧ ... ∧ pn ⇒ q)
SUBST(θ,q)


The rule has n+1 premises (n atomic sentences pi0 and one implication), resulting in the application of θ to the consequent q.

## Unification

Unification is a crucial process in first-order inference, allowing for the identification of substitutions making different logical expressions identical. The UNIFY algorithm returns a unifier for two sentences if one exists.

**Examples of UNIFY:**
- `UNIFY(Knows(John, x), Knows(John, Jane)) = {x/Jane}`
- `UNIFY(Knows(John, x), Knows(y, Bill)) = {x/Bill, y/John}`
- `UNIFY(Knows(John, x), Knows(y, Mother(y))) = {y/John, x/Mother(John)}`
- `UNIFY(Knows(John, x), Knows(x, Elizabeth)) = failure`

## Storage and Retrieval

Underlying TELL, ASK, and ASKVARS functions are more primitive STORE and FETCH functions. Efficient retrieval is achieved through predicate indexing, organizing facts into buckets based on predicates.

### Subsumption Lattice

Queries are organized in a subsumption lattice, forming a structure for creating indices. It allows for efficient retrieval by constructing index keys from queries, ensuring quick access to facts.

**Subsumption Lattice Example:**
- Employs(x,y)
  - Employs(x,Richard)
  - Employs(IBM,y)
  - Employs(IBM,Richard)
  - Employs(x,y)
  - Employs(John,John)
  - Employs(x,John)
  - Employs(x,x)
  - Employs(John,y)

Limiting indices to commonly used queries is essential for efficiency. Various strategies, including fixed and adaptive policies, can be adopted for index creation.

# Summary of Chapter 9: Inference in First-Order Logic

## 9.4 Backward Chaining
- Backward chaining is a logical inference algorithm that works backward from the goal by chaining through rules to find known facts that support the proof.
- It is an AND/OR search, proving the goal by any rule in the knowledge base (OR) and proving all conjuncts in the left-hand side of a clause (AND).

### 9.4.1 Backward-Chaining Algorithm
- FOL-BC-ASK(KB, goal) proves if the knowledge base contains a rule of the form lhs ⇒ goal.
- Implemented as a generator for multiple possible results.
- FOL-BC-OR fetches clauses and calls FOL-BC-AND to prove each conjunct.

### 9.4.2 Logic Programming
- Logic programming embodies the declarative ideal, expressing knowledge in a formal language for problem-solving through inference.
- Prolog is a widely used logic programming language, written in definite clauses notation.

### 9.4.3 Redundant Inference and Infinite Loops
- Prolog's depth-first backward chaining can lead to infinite loops and redundant computations.
- Tabled logic programming addresses this issue by combining goal-directedness with dynamic-programming efficiency.

### 9.4.4 Database Semantics of Prolog
- Prolog uses database semantics with unique names and the closed-world assumption.
- It sacrifices expressiveness for efficiency, making it suitable for certain applications.

### 9.4.5 Constraint Logic Programming
- Constraint Logic Programming (CLP) allows variables to be constrained, providing solutions as sets of constraints.
- CLP systems combine constraint satisfaction algorithms, logic programming, and deductive databases.

## Conclusion
- Backward chaining is powerful but has limitations, leading to issues like redundant inference and infinite loops.
- Prolog is a popular logic programming language with database semantics.
- Constraint Logic Programming extends logic programming to handle constraints on variables.

# Summary: Inference in First-Order Logic

## 9.5 Resolution

### Conjunctive Normal Form for First-Order Logic
- Convert sentences to conjunctive normal form (CNF) – a conjunction of clauses, each a disjunction of literals.
- Eliminate implications, move negation inwards, standardize variables, and skolemize to achieve CNF.
- Skolemization involves removing existential quantifiers by introducing Skolem functions.
- Drop universal quantifiers to obtain a CNF sentence.

### The Resolution Inference Rule
- Resolution rule for first-order clauses extends propositional resolution.
- Two clauses can be resolved if they contain complementary literals, which unify in the first-order case.
- Binary resolution resolves exactly two literals but is incomplete alone; it requires factoring for completeness.

### Example Proofs
- Resolution proves that KB |= α by deriving KB ∧ ¬α unsatisfiable.
- Two example proofs demonstrate the resolution process for crime and Skolemization.

### Completeness of Resolution
- Resolution is refutation-complete, proving that if a set of sentences is unsatisfiable, resolution will derive a contradiction.
- The completeness proof involves Herbrand's theorem, ground resolution, and a lifting lemma.

### Equality
- Three approaches for handling equality in first-order logic: axiomatize equality, add inference rules (demodulation, paramodulation), or use equational unification within an extended unification algorithm.

### Resolution Strategies
- **Unit Preference:** Prefers resolutions involving unit clauses for efficiency.
- **Best-First Search:** Utilizes a heuristic function to prioritize lighter clauses in a best-first search strategy.
- **Otter Theorem Prover:** An example of a system using best-first search with a weight-based heuristic for clause selection.

*Note: This summary provides a concise overview of key concepts in the context of inference in first-order logic.*



