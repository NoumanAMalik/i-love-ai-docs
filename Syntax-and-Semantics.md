# Syntax and Semantics of First-Order Logic

First-order logic (FOL), also known as predicate logic or first-order predicate calculus, is a framework for constructing formal proofs and models in mathematics, computer science, and artificial intelligence. It extends propositional logic with quantifiers and predicates, allowing for the expression of statements about objects and their relationships. Understanding the syntax and semantics of FOL is crucial for its application in various domains.

## Syntax of First-Order Logic

The syntax of FOL defines the formal structure of expressions without considering their meaning. It includes symbols, terms, formulas, and rules for constructing well-formed formulas (WFFs).

### Symbols

1. **Variables**: Represent objects in the domain of discourse.
2. **Constants**: Denote specific objects in the domain.
3. **Predicates**: Represent properties of objects or relations among objects.
4. **Functions**: Map objects to other objects.
5. **Logical Connectives**: Include ∧ (and), ∨ (or), ¬ (not), → (implies), and ↔ (if and only if).
6. **Quantifiers**: Include ∀ (for all) and ∃ (there exists).

### Terms

Terms represent objects in the domain. A term can be a variable, a constant, or a function applied to a sequence of terms.

### Formulas

Formulas express statements that can be true or false. An atomic formula consists of a predicate applied to a sequence of terms. Complex formulas can be built from atomic formulas using logical connectives and quantifiers.

### Rules for Well-Formed Formulas

1. **Atomic Formulas**: If P is an n-ary predicate and \(t_1, t_2, \ldots, t_n\) are terms, then \(P(t_1, t_2, \ldots, t_n)\) is a WFF.
2. **Negation**: If φ is a WFF, then ¬φ is a WFF.
3. **Binary Connectives**: If φ and ψ are WFFs, then (φ ∧ ψ), (φ ∨ ψ), (φ → ψ), and (φ ↔ ψ) are WFFs.
4. **Quantification**: If φ is a WFF and x is a variable, then ∀xφ and ∃xφ are WFFs.

## Semantics of First-Order Logic

The semantics of FOL provides the meaning of syntactically correct formulas by interpreting them with respect to a domain of discourse.

### Interpretation

An interpretation specifies:
1. **Domain of Discourse**: A non-empty set of objects.
2. **Assignment**: Maps constants to objects, predicates to sets of tuples (representing relations), and functions to mappings between objects.
3. **Variable Assignment**: A function that assigns objects to variables.

### Satisfaction

A formula φ is satisfied by an interpretation I under a variable assignment V (written as I, V ⊨ φ) if φ is true in I when the variables are interpreted according to V.

### Models

An interpretation I is a model of a formula φ if I satisfies φ for every variable assignment. If I is a model of φ, we say that I models φ, written as I ⊨ φ.

### Quantifiers

- **Universal Quantification**: ∀xφ is true under interpretation I if φ is true for every assignment of an object in the domain to x.
- **Existential Quantification**: ∃xφ is true under interpretation I if φ is true for at least one assignment of an object in the domain to x.

## Logical Consequence and Equivalence

- **Logical Consequence**: A formula φ is a logical consequence of a set of formulas Γ (written as Γ ⊨ φ) if every model of Γ is also a model of φ.
- **Logical Equivalence**: Two formulas φ and ψ are logically equivalent (φ ≡ ψ) if they are true in exactly the same models.

## Conclusion

The syntax and semantics of First-Order Logic lay the groundwork for expressing and reasoning about properties of objects and their relationships in a precise and unambiguous manner. FOL's expressive power makes it a cornerstone of mathematical logic, theoretical computer science, and artificial intelligence research.
