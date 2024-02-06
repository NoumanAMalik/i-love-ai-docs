# Chapter 8: First-Order Logic

## Overview
Chapter 8 introduces **First-Order Logic (FOL)**, an extension of propositional logic, for representing complex relationships between objects. It overcomes limitations of propositional logic and explores the combination of formalism and expressiveness.

## 8.1 Representation Revisited

### 8.1.1 Language of Thought
- Discusses representation languages, contrasting programming and propositional logic.
- Explores expressive power of natural languages.

### 8.1.2 Combining the best of formal and natural languages
- Proposes combining formalism with natural language's expressiveness.
- Introduces FOL elements: objects, relations, functions.

### 8.1.3 Ontological and Epistemological Commitments
- Explores FOL's ontological commitment to objects and relations.
- Discusses three states of knowledge in FOL.

### 8.1.4 Special-Purpose Logics
- Covers special-purpose logics like temporal, fuzzy, and higher-order logic.
- Describes their unique commitments.

### 8.1.5 Probability Theory
- Introduces probability theory allowing degrees of belief.
- Differentiates belief in probability theory and truth in fuzzy logic.

# Chapter 8.2: Syntax and Semantics of First-Order Logic

## 8.2.1 Models for First-Order Logic
In first-order logic, models represent possible worlds and consist of objects, relations, and functions. The domain of a model is the set of objects it contains, and relationships between objects are captured through relations and functions.

### 8.2.2 Symbols and Interpretations
- The syntax of first-order logic involves constant symbols (objects), predicate symbols (relations), and function symbols.
- Models include an interpretation specifying how symbols map to objects, relations, and functions.
- Intended interpretations, like mapping names to specific objects, illustrate the flexibility of models.

### 8.2.3 Terms
- Terms are logical expressions referring to objects. Constant symbols and function symbols, with their arguments, create terms.
- The interpretation of terms is straightforward, determined by the objects and functions in the model.

### 8.2.4 Atomic Sentences
- Atomic sentences, or atoms, state facts using predicate symbols and terms as arguments.
- An atomic sentence is true in a model if the relation referred to by the predicate symbol holds among the specified objects.

### 8.2.5 Complex Sentences
- Logical connectives (¬, ∧, ∨, ⇒) combine atomic sentences to form complex sentences.
- Examples of complex sentences demonstrate how to express relationships and conditions in first-order logic.

# First-Order Logic: Quantifiers and Equality

## 8.2.6 Quantifiers

- **Universal Quantification (∀)**
  - Express properties for entire collections of objects.
  - ∀x King(x) ⇒ Person(x) means "For all x, if x is a king, then x is a person."
  - Variables (e.g., x) are used as placeholders.
  - ∃x represents "There exists an x such that..."
- **Existential Quantification (∃)**
  - Express statements about some object without naming it.
  - ∃x Crown(x)∧OnHead(x, John) means "There exists an x such that x is a crown on John's head."

## Nested Quantifiers

- Combine quantifiers for more complex statements.
- Examples:
  - "Brothers are siblings": ∀x ∀y Brother(x, y) ⇒ Sibling(x, y).
  - "Everybody loves somebody": ∀x ∃y Loves(x, y).
  - "There is someone who is loved by everyone": ∃y ∀x Loves(x, y).

## Connections between ∀ and ∃

- Negation connects universal and existential quantifiers.
- De Morgan's rules apply:
  - ¬∃x P ≡ ∀x ¬P
  - ¬∀x P ≡ ∃x ¬P

## 8.2.7 Equality

- Use the equality symbol to signify that two terms refer to the same object.
- Examples:
  - Father(John) = Henry
  - ∃x, y Brother(x, Richard)∧Brother(y, Richard)∧ ¬(x=y) for stating Richard has at least two brothers.

## 8.2.8 Database Semantics

- Alternative semantics for first-order logic used in database systems.
- Unique-names assumption: Every constant symbol refers to a distinct object.
- Closed-world assumption: Atomic sentences not known to be true are false.
- Domain closure: Models contain only domain elements named by constant symbols.
- Useful for expressing knowledge when certain about object identity and facts are known.

*Note:* There is no one "correct" semantics for logic, and the choice depends on the specific requirements and use cases.

# Summary: Using First-Order Logic

## 8.3 Using First-Order Logic

### 8.3.1 Assertions and queries in first-order logic
- Sentences added using TELL, assertions.
- Queries made using ASK.
- Quantified queries using ASKVARS for substitution lists.

### 8.3.2 The kinship domain
- Domain of family relationships.
- Unary and binary predicates represent relationships.
- Axioms define relationships and functions in terms of others.

### 8.3.3 Numbers, sets, and lists
- Natural numbers defined using Peano axioms.
- Sets and their operations defined with axioms.
- Lists similar to sets but ordered and allow repeated elements.

### 8.3.4 The wumpus world
- Logical representation of the wumpus world.
- Axioms for percepts, actions, and environment properties.
- Use of quantification over time and space.

## 8.4 Knowledge Engineering in First-Order Logic
- Axioms as definitions and facts.
- Theorems and their role in reducing computational cost.
- Building domain representation incrementally.

## Knowledge Engineering Process

### 1. Identify the Questions
- Define specific queries the knowledge base will answer.
- Determine the range of questions based on the task at hand.

### 2. Assemble Relevant Knowledge
- Extract domain knowledge from experts.
- This step, known as knowledge acquisition, aims to informally understand the domain's scope.

### 3. Decide on Vocabulary
- Translate domain-level concepts into logic-level names.
- Make decisions about representation style, e.g., objects or predicates.

### 4. Encode General Knowledge
- Write down axioms that formalize general knowledge about the domain.
- Ensure clarity and accuracy in defining terms.

### 5. Encode Problem Instance
- Use the established ontology to represent a specific problem instance.
- Specify the details of the particular scenario within the logical framework.

### 6. Pose Queries
- Utilize an inference procedure to ask questions and derive answers.
- Realize the value of the knowledge base by gaining insights or solutions.

### 7. Debug and Evaluate
- Identify and correct errors in the knowledge base.
- Evaluate performance using test queries to ensure correctness.

## Electronic Circuits Domain

### Identify Questions
- Analyze circuit functionality and structure.
- Examine specific aspects like connections between gates.

### Assemble Relevant Knowledge
- Understand digital circuits composed of wires and gates.
- Recognize the flow of signals along wires to input terminals of gates.

### Decide on Vocabulary
- Select functions, predicates, and constants to represent circuits, terminals, signals, and gates.
- Choose a representation style, e.g., using functions to denote gate types.

### Encode General Knowledge
- Define axioms capturing behavior based on gate types and connections.
- Include rules for specific gate types (AND, OR, XOR, NOT) and their relationships.

### Encode Problem Instance
- Represent a specific circuit (e.g., one-bit full adder) using the defined ontology.
- Specify types of gates, connections, and overall structure.

### Pose Queries
- Ask questions about the circuit's behavior, e.g., input combinations resulting in specific output values.
- Request a complete input-output table to verify correctness.

### Debug Knowledge Base
- Identify and correct errors by perturbing the knowledge base and analyzing the impact on queries.
- Ensure that axioms and rules accurately represent desired behavior.

### Bibliographical and Historical Notes // Not Included

