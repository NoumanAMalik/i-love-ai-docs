# Logical Agents

In this chapter, we design agents capable of forming representations of a complex world, using inference to derive new representations about the world, and deducing actions based on these representations.

Humans possess knowledge that aids them in their actions. In AI, knowledge-based agents utilize reasoning over an internal representation of knowledge to decide on actions.

Problem-solving agents, as discussed in Chapters 3 and 4, have limited and inflexible knowledge. They lack general facts and utilize atomic representations.

Chapter 5 introduced a factored representation, a step towards more domain-independent operations and efficient algorithms.

In this chapter, we extend this approach by developing logic as a general class of representations for knowledge-based agents. These agents can manipulate information for various purposes, including tasks unrelated to immediate needs.

## 7.1 Knowledge-Based Agents

The central component of a knowledge-based agent is its knowledge base (KB), a set of sentences representing assertions about the world. These sentences, expressed in a knowledge representation language, can be axioms or derived from other sentences through inference.

![Figure 7.1: Knowledge-Based Agent](image_link)

A knowledge-based agent program consists of TELLing the KB about percepts, ASKing the KB for actions, and updating the KB with the chosen action.

## 7.2 The Wumpus World

The Wumpus World environment provides a platform for knowledge-based agents. It's a cave with rooms connected by passageways, containing hazards like the wumpus, bottomless pits, and gold.

The agent's performance measure, environment, actuators, and sensors are defined, allowing for deterministic, discrete, sequential, and partially observable interactions.

![Figure 7.2: Wumpus World](image_link)

The agent explores the environment by reasoning about percepts and inferring the presence of hazards or goals, ensuring safe navigation and gold retrieval.

![Figure 7.3: Agent Progress (Step 1)](image_link)

Logical reasoning ensures the correctness of the agent's conclusions based on available information, laying the foundation for building logical agents capable of representing and reasoning about complex environments.

## 7.3 Logic

This section summarizes the fundamental concepts of logical representation and reasoning. These beautiful ideas are independent of any of logic’s particular forms. We therefore postpone the technical details of those forms until the next section, using instead the familiar example of ordinary arithmetic.

In Section 7.1, we said that knowledge bases consist of sentences. These sentences are expressed according to the syntax of the representation language, which specifies all the sentences that are well formed. The notion of syntax is clear enough in ordinary arithmetic: **“x+y = 4”** is a well-formed sentence, whereas **“x4y+ =”** is not.

A logic must also define the semantics, or meaning, of sentences. The semantics defines the truth of each sentence with respect to each possible world. For example, the semantics for arithmetic specifies that the sentence **“x+y=4”** is true in a world where x is 2 and y is 2, but false in a world where x is 1 and y is 1. In standard logics, every sentence must be either true or false in each possible world—there is no “in between.”

When we need to be precise, we use the term model in place of **“possible world.”** Whereas possible worlds might be thought of as (potentially) real environments that the agent might or might not be in, models are mathematical abstractions, each of which has a fixed truth value (true or false) for every relevant sentence. If a sentence α is true in model m, we say that m satisfies α or sometimes m is a model of α. We use the notation M(α) to mean the set of all models of α.

Now that we have a notion of truth, we are ready to talk about logical reasoning. This involves the relation of logical entailment between sentences—the idea that a sentence follows logically from another sentence. In mathematical notation, we write **α |= β** to mean that the sentence α entails the sentence β. The formal definition of entailment is this: α |= β if and only if, in every model in which α is true, β is also true. Using the notation just introduced, we can write α |= β if and only if M(α) ⊆ M(β). The relation of entailment is familiar from arithmetic; we are happy with the idea that the sentence **x = 0** entails the sentence **xy = 0**. Obviously, in any model where x is zero, it is the case that xy is zero (regardless of the value of y).

We can apply the same kind of analysis to the wumpus-world reasoning example given in the preceding section. Consider the situation in Figure 7.3(b): the agent has detected nothing in [1,1] and a breeze in [2,1]. These percepts, combined with the agent’s knowledge of the rules of the wumpus world, constitute the KB. The agent is interested in whether the adjacent squares [1,2], [2,2], and [3,1] contain pits. Each of the three squares might or might not contain a pit, so (ignoring other aspects of the world for now) there are 2^3 = 8 possible models. These eight models are shown in Figure 7.5.

The KB can be thought of as a set of sentences or as a single sentence that asserts all the individual sentences. The KB is false in models that contradict what the agent knows—for example, the KB is false in any model in which [1,2] contains a pit, because there is no breeze in [1,1]. There are in fact just three models in which the KB is true, and these are shown surrounded by a solid line in Figure 7.5. Now let us consider two possible conclusions: 
α1 = “There is no pit in [1,2].” α2 = “There is no pit in [2,2].”

We have surrounded the models of α1 and α2 with dotted lines in Figures 7.5(a) and 7.5(b), respectively. By inspection, we see the following: 
- in every model in which KB is true, α1 is also true. Hence, KB |= α1: there is no pit in [1,2]. 
- In some models in which KB is true, α2 is false. Hence, KB does not entail α2: the agent cannot conclude that there is no pit in [2,2]. (Nor can it conclude that there is a pit in [2,2]).

The preceding example not only illustrates entailment but also shows how the definition of entailment can be applied to derive conclusions—that is, to carry out logical inference.

The inference algorithm illustrated in Figure 7.5 is called model checking, because it enumerates all possible models to check that α is true in all models in which KB is true, that is, that M(KB) ⊆ M(α).


## 7.4 Propositional Logic: A Very Simple Logic

We now present propositional logic. We describe its syntax (the structure of sentences) and Propositional logic its semantics (the way in which the truth of sentences is determined). From these, we derive a simple, syntactic algorithm for logical inference that implements the semantic notion of entailment. Everything takes place, of course, in the wumpus world.

### Syntax

The syntax of propositional logic defines the allowable sentences. The atomic sentences Atomic sentences consist of a single proposition symbol. Each such symbol stands for a proposition that can Proposition symbol be true or false. We use symbols that start with an uppercase letter and may contain other letters or subscripts, for example: `P`, `Q`, `R`, `W1,3`, and `FacingEast`. The names are arbitrary but are often chosen to have some mnemonic value—we use `W1,3` to stand for the proposition that the wumpus is in [1,3]. (Remember that symbols such as `W1,3` are atomic, i.e., `W`, `1`, and `3` are not meaningful parts of the symbol.) There are two proposition symbols with fixed meanings: `True` is the always-true proposition and `False` is the always-false proposition.

Complex sentences are constructed from simpler sentences, using parentheses and operators Complex sentences called logical connectives. There are five connectives in common use: 

- ¬ (not). A sentence such as ¬W1,3 is called the negation of W1,3. A literal is either an Negation atomic sentence (a positive literal) or a negated atomic sentence (a negative literal).
- ∧ (and). A sentence whose main connective is ∧, such as W1,3 ∧P3,1, is called a conjunction; its parts are the conjuncts. (The ∧ looks like an “A” for “And.”)
- ∨ (or). A sentence whose main connective is ∨, such as (W1,3 ∧P3,1)∨W2,2, is a disjunction; its parts are disjuncts—in this example, (W1,3 ∧P3,1) and W2,2.
- ⇒ (implies). A sentence such as (W1,3 ∧P3,1) ⇒ ¬W2,2 is called an implication (or conditional). Its premise or antecedent is (W1,3 ∧P3,1), and its conclusion or consequent Premise is ¬W2,2. Implications are also known as rules or if–then statements.
- ⇔ (if and only if). The sentence W1,3 ⇔ ¬W2,2 is a biconditional.

Figure 7.7 gives a formal grammar of propositional logic. (BNF notation is explained on page 1081.) The BNF grammar is augmented with an operator precedence list to remove ambiguity when multiple operators are used. The “not” operator (¬) has the highest precedence, which means that in the sentence ¬A∧B the ¬ binds most tightly, giving us the equivalent of (¬A)∧B rather than ¬(A∧B). (The notation for ordinary arithmetic is the same: −2+4 is 2, not –6.) When appropriate, we also use parentheses and square brackets to clarify the intended sentence structure and improve readability.

### Semantics

Having specified the syntax of propositional logic, we now specify its semantics. The semantics defines the rules for determining the truth of a sentence with respect to a particular Truth value model. In propositional logic, a model simply sets the truth value—true or false—for every proposition symbol. For example, if the sentences in the knowledge base make use of the proposition symbols P1,2, P2,2, and P3,1, then one possible model is:

m1 = {P1,2 =false, P2,2 =false, P3,1 =true}.

With three proposition symbols, there are 2^3 = 8 possible models—exactly those depicted in Figure 7.5. Notice, however, that the models are purely mathematical objects with no necessary connection to wumpus worlds. P1,2 is just a symbol; it might mean “there is a pit in [1,2]” or “I’m in Paris today and tomorrow.”

The semantics for propositional logic must specify how to compute the truth value of any sentence, given a model. This is done recursively. All sentences are constructed from atomic sentences and the five connectives; therefore, we need to specify how to compute the truth of atomic sentences and how to compute the truth of sentences formed with each of the five connectives. Atomic sentences are easy:

- `True` is true in every model and `False` is false in every model.
- The truth value of every other proposition symbol must be specified directly in the model. For example, in the model `m1` given earlier, `P1,2` is false.

## 7.5 Propositional Theorem Proving

So far, we have shown how to determine entailment by model checking: enumerating models and showing that the sentence must hold in all models. In this section, we show how entailment can be done by theorem proving—applying rules of inference directly to the sentences in our knowledge base to construct a proof of the desired sentence without consulting models. If the number of models is large but the length of the proof is short, then theorem proving can be more efficient than model checking.

Before we plunge into the details of theorem-proving algorithms, we will need some additional concepts related to entailment. The first concept is logical equivalence: two sentences α and β are logically equivalent if they are true in the same set of models. We write this as α ≡ β. (Note that ≡ is used to make claims about sentences, while ⇔ is used as part of a sentence.) For example, we can easily show (using truth tables) that P∧Q and Q∧P are logically equivalent; other equivalences are shown in Figure 7.11. These equivalences play much the same role in logic as arithmetic identities do in ordinary mathematics. An alternative definition of equivalence is as follows: any two sentences α and β are equivalent if and only if each of them entails the other:

α ≡ β if and only if α |= β and β |= α.

The second concept we will need is validity. A sentence is valid if it is true in all models. For example, the sentence P∨ ¬P is valid. Valid sentences are also known as tautologies—they are necessarily true. Because the sentence True is true in all models, every valid sentence is logically equivalent to True. What good are valid sentences? From our definition of entailment, we can derive the deduction theorem, which was known to the ancient Greeks:

For any sentences α and β, α |= β if and only if the sentence (α ⇒ β) is valid. Hence, we can decide if α |= β by checking that (α ⇒ β) is true in every model—which is essentially what the inference algorithm in Figure 7.10 does—or by proving that (α ⇒ β) is equivalent to True. Conversely, the deduction theorem states that every valid implication sentence describes a legitimate inference.

The final concept we will need is satisfiability. A sentence is satisfiable if it is true in, or satisfied by, some model. For example, the knowledge base given earlier, (R1 ∧R2 ∧ R3 ∧ R4 ∧ R5), is satisfiable because there are three models in which it is true, as shown in Figure 7.9. Satisfiability can be checked by enumerating the possible models until one is found that satisfies the sentence. The problem of determining the satisfiability of sentences in propositional logic—the SAT problem—was the first problem proved to be NP-complete. Many problems in computer science are really satisfiability problems. For example, all the constraint satisfaction problems in Chapter 5 ask whether the constraints are satisfiable by some assignment.

Validity and satisfiability are of course connected: α is valid iff ¬α is unsatisfiable; contrapositively, α is satisfiable iff ¬α is not valid. We also have the following useful result:

α |= β if and only if the sentence (α∧ ¬β) is unsatisfiable.

Proving β from α by checking the unsatisfiability of (α ∧ ¬β) corresponds exactly to the standard mathematical proof technique of reductio ad absurdum (literally, “reduction to an absurd thing”). It is also called proof by refutation or proof by contradiction. One assumes a sentence β to be false and shows that this leads to a contradiction with known axioms α. This contradiction is exactly what is meant by saying that the sentence (α∧ ¬β) is unsatisfiable.

### Inference and proofs

This section covers inference rules that can be applied to derive a proof—a chain of conclusions that leads to the desired goal. The best-known rule is called Modus Ponens (Latin for "mode that affirms") and is written: α ⇒ β, α
β


The notation means that, whenever any sentences of the form α ⇒ β and α are given, then the sentence β can be inferred. For example, if (WumpusAhead ∧ WumpusAlive) ⇒ Shoot and (WumpusAhead∧ WumpusAlive) are given, then Shoot can be inferred.

Another useful inference rule is And-Elimination, which says that, from a conjunction, any of the conjuncts can be inferred: α∧β
α


For example, from (WumpusAhead∧WumpusAlive), WumpusAlive can be inferred.

By considering the possible truth values of α and β, one can easily show once and for all that Modus Ponens and And-Elimination are sound. These rules can then be used in any particular instances where they apply, generating sound inferences without the need for enumerating models.

All of the logical equivalences in Figure 7.11 can be used as inference rules. For example, the equivalence for biconditional elimination yields the two inference rules: α ⇔ β
(α ⇒ β)∧(β ⇒ α) and (α ⇒ β)∧(β ⇒ α)
α ⇔ β

Not all inference rules work in both directions like this. For example, we cannot run Modus Ponens in the opposite direction to obtain α ⇒ β and α from β.

Let us see how these inference rules and equivalences can be used in the wumpus world. We start with the knowledge base containing R1 through R5 and show how to prove ¬P1,2, that is, there is no pit in [1,2]:

1. Apply biconditional elimination to R2 to obtain R6 : (B1,1 ⇒ (P1,2 ∨P2,1)) ∧ ((P1,2 ∨P2,1) ⇒ B1,1).
2. Apply And-Elimination to R6 to obtain R7 : ((P1,2 ∨P2,1) ⇒ B1,1).
3. Logical equivalence for contrapositives gives R8 : (¬B1,1 ⇒ ¬(P1,2 ∨P2,1)).
4. Apply Modus Ponens with R8 and the percept R4 (i.e., ¬B1,1), to obtain R9 : ¬(P1,2 ∨P2,1).
5. Apply De Morgan’s rule, giving the conclusion R10 : ¬P1,2 ∧ ¬P2,1 .

That is, neither [1,2] nor [2,1] contains a pit.

Any of the search algorithms in Chapter 3 can be used to find a sequence of steps that constitutes a proof like this. We just need to define a proof problem as follows:

- **INITIAL STATE:** the initial knowledge base.
- **ACTIONS:** the set of actions consists of all


###  Horn clauses and definite clauses

The completeness of resolution makes it a very important inference method. In many practical situations, however, the full power of resolution is not needed. Some real-world knowledge bases satisfy certain restrictions on the form of sentences they contain, which enables them to use a more restricted and efficient inference algorithm.

One such restricted form is the definite clause, which is a disjunction of literals of which exactly one is positive. For example, the clause `(¬L1,1 ∨ ¬Breeze∨B1,1)` is a definite clause, whereas `(¬B1,1 ∨P1,2 ∨P2,1)` is not, because it has two positive clauses.

Slightly more general is the Horn clause, which is a disjunction of literals of which at most one is positive. So all definite clauses are Horn clauses, as are clauses with no positive literals; these are called goal clauses. Horn clauses are closed under resolution: if you resolve two Horn clauses, you get back a Horn clause. One more class is the k-CNF sentence, which is a CNF sentence where each clause has at most k literals.

Knowledge bases containing only definite clauses are interesting for three reasons:

1. Every definite clause can be written as an implication whose premise is a conjunction of positive literals and whose conclusion is a single positive literal. (See Exercise 7.DISJ.) For example, the definite clause `(¬L1,1 ∨¬Breeze∨B1,1)` can be written as the implication `(L1,1∧Breeze) ⇒ B1,1`. In the implication form, the sentence is easier to understand: it says that if the agent is in [1,1] and there is a breeze percept, then [1,1] is breezy. In Body Horn form, the premise is called the body and the conclusion is called the head. A sentence consisting of a single positive literal, such as `L1,1`, is called a fact. It too can be written in implication form as `True ⇒ L1,1`, but it is simpler to write just `L1,1`.

2. Inference with Horn clauses can be done through the forward-chaining and backward-chaining algorithms, which we explain next. Both of these algorithms are natural, in that the inference steps are obvious and easy for humans to follow. This type of inference is the basis for logic programming, which is discussed in Chapter 9.

3. Deciding entailment with Horn clauses can be done in time that is linear in the size of the knowledge base—a pleasant surprise.

### Forward and backward chaining

The forward-chaining algorithm `PL-FC-ENTAILS?(KB, q)` determines if a single proposition symbol q—the query—is entailed by a knowledge base of definite clauses. It begins from known facts (positive literals) in the knowledge base. If all the premises of an implication are known, then its conclusion is added to the set of known facts. For example, if `L1,1` and `Breeze` are known and `(L1,1 ∧Breeze) ⇒ B1,1` is in the knowledge base, then `B1,1` can be added. This process continues until the query `q` is added or until no further inferences can be made. The algorithm is shown in Figure 7.15; the main point to remember is that it runs in linear time.

The best way to understand the algorithm is through an example and a picture. Figure 7.16(a) shows a simple knowledge base of Horn clauses with `A` and `B` as known facts. Figure 7.16(b) shows the same knowledge base drawn as an AND–OR graph (see Chapter 4). In AND–OR graphs, multiple edges joined by an arc indicate a conjunction—every edge must be proved—while multiple edges without an arc indicate a disjunction—any edge can be proved. It is easy to see how forward chaining works in the graph. The known leaves (here, `A` and `B`) are set, and inference propagates up the graph as far as possible. Wherever a conjunction appears, the propagation waits until all the conjuncts are known before proceeding. The reader is encouraged to work through the example in detail.

It is easy to see that forward chaining is sound: every inference is essentially an application of Modus Ponens. Forward chaining is also complete: every entailed atomic sentence will be derived. The easiest way to see this is to consider the final state of the inferred table (after the algorithm reaches a fixed point where no new inferences are possible). The table contains true for each symbol inferred during the process, and false for all other symbols. We can view the table as a logical model; moreover, every definite clause in the original KB is true in this model.

```python
function PL-FC-ENTAILS?(KB, q) returns true or false
inputs: KB, the knowledge base, a set of propositional definite clauses
        q, the query, a proposition symbol
count←a table, where count[c] is initially the number of symbols in clause c’s premise
inferred←a table, where inferred[s] is initially false for all symbols
queue←a queue of symbols, initially symbols known to be true in KB
while queue is not empty do
    p←POP(queue)
    if p = q then return true
    if inferred[p] = false then
        inferred[p]←true
        for each clause c in KB where p is in c.PREMISE do
            decrement count[c]
            if count[c] = 0 then add c.CONCLUSION to queue
return false
```

The backward-chaining algorithm, as its name suggests, works backward from the query. If the query q is known to be true, then no work is needed. Otherwise, the algorithm finds those implications in the knowledge base whose conclusion is q. If all the premises of one of those implications can be proved true (by backward chaining), then q is true. When applied to the query Q in Figure 7.16, it works back down the graph until it reaches a set of known facts, A and B, that forms the basis for a proof. The algorithm is essentially identical to the AND-OR-GRAPH-SEARCH algorithm in Figure 4.11. As with forward chaining, an efficient implementation runs in linear time.

Backward chaining is a form of goal-directed reasoning. It is useful for answering specific questions such as "What shall I do now?" and "Where are my keys?" Often, the cost of backward chaining is much less than linear in the size of the knowledge base, because the process touches only relevant facts.


## 7.6 Effective Propositional Model Checking

In this section, we describe two families of efficient algorithms for general propositional inference based on model checking: one approach based on backtracking search, and one on local hill-climbing search. These algorithms are part of the “technology” of propositional logic. This section can be skimmed on a first reading of the chapter.

The algorithms we describe are for checking satisfiability: the SAT problem. (As noted in Section 7.5, testing entailment, α |= β, can be done by testing unsatisfiability of α∧ ¬β.) We mentioned on page 241 the connection between finding a satisfying model for a logical sentence and finding a solution for a constraint satisfaction problem, so it is perhaps not surprising that the two families of propositional satisfiability algorithms closely resemble the backtracking algorithms of Section 5.3 and the local search algorithms of Section 5.4. They are, however, extremely important in their own right because so many combinatorial problems in computer science can be reduced to checking the satisfiability of a propositional sentence. Any improvement in satisfiability algorithms has huge consequences for our ability to handle complexity in general.

###  A complete backtracking algorithm

The first algorithm we consider is often called the Davis–Putnam algorithm, after the sem- Davis–Putnam algorithm inal paper by Martin Davis and Hilary Putnam (1960). The algorithm is in fact the version described by Davis, Logemann, and Loveland (1962), so we will call it DPLL after the initials of all four authors. DPLL takes as input a sentence in conjunctive normal form—a set of clauses. Like BACKTRACKING-SEARCH and TT-ENTAILS?, it is essentially a recursive, depth-first enumeration of possible models. It embodies three improvements over the simple scheme of TT-ENTAILS?:

- **Early termination:** The algorithm detects whether the sentence must be true or false, even with a partially completed model. A clause is true if any literal is true, even if the other literals do not yet have truth values; hence, the sentence as a whole could be judged true even before the model is complete. For example, the sentence (A ∨ B) ∧ (A∨C) is true if A is true, regardless of the values of B and C. Similarly, a sentence is false if any clause is false, which occurs when each of its literals is false. Again, this can occur long before the model is complete. Early termination avoids examination of entire subtrees in the search space.
- **Pure symbol heuristic:** A pure symbol is a symbol that always appears with the same Pure symbol “sign” in all clauses. For example, in the three clauses(A∨¬B), (¬B∨¬C), and (C∨A), the symbol A is pure because only the positive literal appears, B is pure because only the negative literal appears, and C is impure. It is easy to see that if a sentence has a model, then it has a model with the pure symbols assigned so as to make their literals true, because doing so can never make a clause false. Note that, in determining the purity of a symbol, the algorithm can ignore clauses that are already known to be true in the model constructed so far. For example, if the model contains B=false, then the clause (¬B ∨ ¬C) is already true, and in the remaining clauses C appears only as a positive literal; therefore C becomes pure.
- **Unit clause heuristic:** A unit clause was defined earlier as a clause with just one literal. In the context of DPLL, it also means clauses in which all literals but one are already assigned false by the model. For example, if the model contains B=true, then (¬B∨ ¬C)simplifies to ¬C, which is a unit clause. Obviously, for this clause to be true,C must be set to false. The unit clause heuristic assigns all such symbols before branching on the remainder. One important consequence of the heuristic is that any attempt to prove (by refutation) a literal that is already in the knowledge base will succeed immediately (Exercise 7.KNOW). Notice also that assigning one unit clause can create another unit clause—for example, when C is set to false, (C∨A) becomes a unit clause, causing true to be assigned to A. This “cascade” of forced assignments is called unit propagation. Unit propagation It resembles the process of forward chaining with definite clauses, and indeed, if the CNF expression contains only definite clauses then DPLL essentially replicates forward chaining. (See Exercise 7.DPLL.)

The DPLL algorithm is shown in Figure 7.17, which gives the essential skeleton of the search process without the implementation details.

What Figure 7.17 does not show are the tricks that enable SAT solvers to scale up to large problems. It is interesting that most of these tricks are in fact rather general, and we have seen them before in other guises:

1. **Component analysis** (as seen with Tasmania in CSPs): As DPLL assigns truth values to variables, the set of clauses may become separated into disjoint subsets, called components, that share no unassigned variables. Given an efficient way to detect when this occurs, a solver can gain considerable speed by working on each component separately.
2. **Variable and value ordering** (as seen in Section 5.3.1 for CSPs): Our simple implementation of DPLL uses an arbitrary variable ordering and always tries the value true before false. The degree heuristic (see page 177) suggests choosing the variable that appears most frequently over all remaining clauses.
3. **Intelligent backtracking** (as seen in Section 5.3.3 for CSPs): Many problems that cannot be solved in hours of run time with chronological backtracking can be solved in seconds with intelligent backtracking that backs up all the way to the relevant point of conflict. All SAT solvers that do intelligent backtracking use some form of conflict clause learning to record conflicts so that they won’t be repeated later in the search. Usually a limited-size set of conflicts is kept, and rarely used ones are dropped.
4. **Random restarts** (as seen on page 131 for hill climbing): Sometimes a run appears not to be making progress. In this case, we can start over from the top of the search tree, rather than trying to continue. After restarting, different random choices (in variable and value selection) are made. Clauses that are learned in the first run are retained after the restart and can help prune the search space. Restarting does not guarantee that a solution will be found faster, but it does reduce the variance on the time to solution.
5. **Clever indexing** (as seen in many algorithms): The speedup methods used in DPLL itself, as well as the tricks used in modern solvers, require fast indexing of such things Section 7.6 Effective Propositional Model Checking 253 as “the set of clauses in which variable Xi appears as a positive literal.” This task is complicated by the fact that the algorithms are interested only in the clauses that have not yet been satisfied by previous assignments to variables, so the indexing structures must be updated dynamically as the computation proceeds.

With these enhancements, modern solvers can

## 7.7 Agents Based on Propositional Logic

In this section, we bring together what we have learned so far in order to construct wumpus world agents that use propositional logic. The first step is to enable the agent to deduce, to the extent possible, the state of the world given its percept history. This requires writing down a complete logical model of the effects of actions. We then show how logical inference can be used by an agent in the wumpus world. We also show how the agent can keep track of the world efficiently without going back into the percept history for each inference. Finally, we show how the agent can use logical inference to construct plans that are guaranteed to achieve its goals, provided its knowledge base is true in the actual world.

###  The current state of the world

As stated at the beginning of the chapter, a logical agent operates by deducing what to do from a knowledge base of sentences about the world. The knowledge base is composed of axioms—general knowledge about how the world works—and percept sentences obtained from the agent’s experience in a particular world. In this section, we focus on the problem of deducing the current state of the wumpus world—where am I, is that square safe, and so on.

We began collecting axioms in Section 7.4.3. The agent knows that the starting square contains no pit (¬P1,1) and no wumpus (¬W1,1). Furthermore, for each square, it knows that the square is breezy if and only if a neighboring square has a pit; and a square is smelly if and only if a neighboring square has a wumpus. Thus, we include a large collection of sentences of the following form: B1,1 ⇔ (P1,2 ∨ P2,1)
S1,1 ⇔ (W1,2 ∨ W2,1)


The agent also knows that there is exactly one wumpus. This is expressed in two parts. First, we have to say that there is at least one wumpus: W1,1 ∨ W1,2 ∨ ... ∨ W4,3 ∨ W4,4


Then we have to say that there is at most one wumpus. For each pair of locations, we add a sentence saying that at least one of them must be wumpus-free:
- ¬W1,1 ∨ ¬W1,2
- ¬W1,1 ∨ ¬W1,3
- ... (similar sentences for other pairs)


So far, so good. Now let’s consider the agent’s percepts. We are using S1,1 to mean there is a stench in [1,1]; can we use a single proposition, Stench to mean that the agent perceives a stench? Unfortunately, we can’t: if there was no stench at the previous time step, then ¬Stench would already be asserted, and the new assertion would simply result in a contradiction. The problem is solved when we realize that a percept asserts something only about the current time. Thus, if the time step (as supplied to MAKE-PERCEPT-SENTENCE in Figure 7.1) is 4, then we add Stench4 to the knowledge base, rather than Stench—neatly avoiding any contradiction with ¬Stench3. The same goes for the breeze, bump, glitter, and scream percepts.

The idea of associating propositions with time steps extends to any aspect of the world that changes over time. For example, the initial knowledge base includes L0_1,1—the agent is in square [1,1] at time 0—as well as FacingEast0, HaveArrow0, and WumpusAlive0. We use the Fluent noun fluent (from the Latin fluens, flowing) to refer to an aspect of the world that changes. “Fluent” is a synonym for “state variable,” in the sense described in the discussion of factored representations in Section 2.4.7 on page 76. Symbols associated with permanent aspects of the world do not need a time superscript and are sometimes called atemporal variables.

We can connect stench and breeze percepts directly to the properties of the squares where they are experienced as follows. For any time step t and any square [x, y], we assert:
- L_t_x,y ⇒ (Breezet ⇔ Bx,y)
- L_t_x,y ⇒ (Stencht ⇔ Sx,y)


Now, of course, we need axioms that allow the agent to keep track of fluents such as L_t_x,y. These fluents change as the result of actions taken by the agent, so, in the terminology of Chapter 3, we need to write down the transition model of the wumpus world as a set of logical sentences.

## Summary

We have introduced knowledge-based agents and have shown how to define a logic with which such agents can reason about the world. The main points are as follows:

- Intelligent agents need knowledge about the world in order to reach good decisions.
- Knowledge is contained in agents in the form of **sentences** in a **knowledge representation language** that are stored in a **knowledge base**.
- A knowledge-based agent is composed of a knowledge base and an inference mechanism. It operates by storing sentences about the world in its knowledge base, using the inference mechanism to infer new sentences, and using these sentences to decide what action to take.
- A representation language is defined by its **syntax**, which specifies the structure of sentences, and its **semantics**, which defines the **truth** of each sentence in each **possible world** or **model**.
- The relationship of **entailment** between sentences is crucial to our understanding of reasoning. A sentence α entails another sentence β if β is true in all worlds where α is true. Equivalent definitions include the **validity** of the sentence α ⇒ β and the **unsatisfiability** of the sentence α∧ ¬β.
- Inference is the process of deriving new sentences from old ones. **Sound** inference algorithms derive only sentences that are entailed; **complete** algorithms derive all sentences that are entailed.
- **Propositional logic** is a simple language consisting of **proposition symbols** and **logical connectives**. It can handle propositions that are known to be true, known to be false, or completely unknown.
- The set of possible models, given a fixed propositional vocabulary, is finite, so entailment can be checked by enumerating models. Efficient model-checking inference algorithms for propositional logic include backtracking and local search methods and can often solve large problems quickly.
- **Inference rules** are patterns of sound inference that can be used to find proofs. The resolution rule yields a complete inference algorithm for knowledge bases that are expressed in **conjunctive normal form**. **Forward chaining** and **backward chaining** are very natural reasoning algorithms for knowledge bases in **Horn form**.
- Local search methods such as WALKSAT can be used to find solutions. Such algorithms are sound but not complete.
- Logical state estimation involves maintaining a logical sentence that describes the set of possible states consistent with the observation history. Each update step requires inference using the transition model of the environment, which is built from **successor-state axioms** that specify how each **fluent** changes.
- Decisions within a logical agent can be made by SAT solving: finding possible models specifying future action sequences that reach the goal. This approach works only for fully observable or sensorless environments.
- Propositional logic does not scale to environments of unbounded size because it lacks the expressive power to deal concisely with time, space, and universal patterns of relationships among objects.



