# Infernece in First-Order Logic

In this chapter, we define effective procedures for answering questions posed in first-order logic. We describe algorithms that can answer any answerable first-order logic question. Section 9.1 introduces inference rules for quantifiers and shows how to reduce first-order inference to propositional inference, albeit at potentially great expense. Section 9.2 describes how unification can be used to construct inference rules that work directly with first-order sentences. We then discuss three major families of first-order inference algorithms: forward chaining (Section 9.3), backward chaining (Section 9.4), and resolution-based theorem proving (Section 9.5).

## 9.1 Propositional vs. First-Order Inference

One way to do first-order inference is to convert the first-order knowledge base to propositional logic and use propositional inference, which we already know how to do. A first step is eliminating universal quantifiers. For example, suppose our knowledge base contains the standard folkloric axiom that all greedy kings are evil:

∀x King(x)∧Greedy(x) ⇒ Evil(x).

From that we can infer any of the following sentences:
- King(John)∧Greedy(John) ⇒ Evil(John)
- King(Richard)∧Greedy(Richard) ⇒ Evil(Richard)
- King(Father(John))∧Greedy(Father(John)) ⇒ Evil(Father(John)).

In general, the rule of Universal Instantiation (UI for short) says that we can infer any Universal Instantiation sentence obtained by substituting a ground term (a term without variables) for a universally quantified variable.

To write out the inference rule formally, we use the notion of substitutions introduced in Section 8.3. Let SUBST(θ,α) denote the result of applying the substitution θ to the sentence α. Then the rule is written:

∀v α  
SUBST({v/g},α)

for any variable v and ground term g. For example, the three sentences given earlier are obtained with the substitutions {x/John}, {x/Richard}, and {x/Father(John)}.

Similarly, the rule of Existential Instantiation replaces an existentially quantified variable with a single new constant symbol. The formal statement is as follows: for any sentence α, variable v, and constant symbol k that does not appear elsewhere in the knowledge base:

∃v α  
SUBST({v/k},α)

For example, from the sentence:
∃x Crown(x)∧OnHead(x, John)
we can infer the sentence:
Crown(C1)∧OnHead(C1, John)
as long as C1 does not appear elsewhere in the knowledge base.

Whereas Universal Instantiation can be applied many times to the same axiom to produce many different consequences, Existential Instantiation need only be applied once, and then the existentially quantified sentence can be discarded.

### 9.1.1 Reduction to propositional inference

We now show how to convert any first-order knowledge base into a propositional knowledge base. The first idea is that, just as an existentially quantified sentence can be replaced by one instantiation, a universally quantified sentence can be replaced by the set of all possible instantiations. For example, suppose our knowledge base contains just the sentences:

∀x King(x)∧Greedy(x) ⇒ Evil(x)  
King(John)  
Greedy(John)  
Brother(Richard, John).

and that the only objects are John and Richard. We apply UI to the first sentence using all possible substitutions, {x/John} and {x/Richard}. We obtain:

King(John)∧Greedy(John) ⇒ Evil(John)  
King(Richard)∧Greedy(Richard) ⇒ Evil(Richard).

Next, replace ground atomic sentences, such as King(John), with proposition symbols, such as JohnIsKing. Finally, apply any of the complete propositional algorithms in Chapter 7 to obtain conclusions such as JohnIsEvil, which is equivalent to Evil(John).

This technique of propositionalization can be made completely general, as we show in Section 9.5. However, there is a problem: when the knowledge base includes a function symbol, the set of possible ground-term substitutions is infinite! For example, if the knowledge base mentions the Father symbol, then infinitely many nested terms such as Father(Father(Father(John))) can be constructed.

Fortunately, there is a famous theorem due to Jacques Herbrand (1930) to the effect that if a sentence is entailed by the original, first-order knowledge base, then there is a proof involving just a finite subset of the propositionalized knowledge base. Since any such subset has a maximum depth of nesting among its ground terms, we can find the subset by first generating all the instantiations with constant symbols (Richard and John), then all terms of depth 1 (Father(Richard) and Father(John)), then all terms of depth 2, and so on, until we are able to construct a propositional proof of the entailed sentence.

We have sketched an approach to first-order inference via propositionalization that is complete—that is, any entailed sentence can be proved. This is a major achievement, given that the space of possible models is infinite. On the other hand, we do not know until the proof is done that the sentence is entailed! What happens when the sentence is not entailed? Can we tell? Well, for first-order logic, it turns out that we cannot. Our proof procedure can go on and on, generating more and more deeply nested terms, but we will not know whether it is stuck in a hopeless loop or whether the proof is just about to pop out. This is very much like the halting problem for Turing machines. Alan Turing (1936) and Alonzo Church (1936) both proved, in rather different ways, the inevitability of this state of affairs. The question of entailment for first-order logic is semidecidable—that is, algorithms exist that say yes to every entailed sentence, but no algorithm exists that also says no to every nonentailed sentence.

## 9.2 Unification and First-Order Inference

The propositionalization approach generates many unnecessary instantiations of universally quantified sentences. We’d rather have an approach that uses just the one rule, reasoning that {x/John} solves the query Evil(x) as follows: given the rule that greedy kings are evil, find some x such that x is a king and x is greedy, and then infer that this x is evil. More generally, if there is some substitution θ that makes each of the conjuncts of the premise of the implication identical to sentences already in the knowledge base, then we can assert the conclusion of the implication, after applying θ. In this case, the substitution θ={x/John} achieves that aim. Now suppose that instead of knowing Greedy(John), we know that everyone is greedy:

∀y Greedy(y). (9.2)

Then we would still like to be able to conclude that Evil(John), because we know that John is a king (given) and John is greedy (because everyone is greedy). What we need for this to work is to find a substitution for both the variables in the implication sentence and the variables in the sentences that are in the knowledge base. In this case, applying the substitution {x/John, y/John} to the implication premises King(x) and Greedy(x) and the knowledge base sentences King(John) and Greedy(y) will make them identical. Thus, we can infer the consequent of the implication.

This inference process can be captured as a single inference rule that we call Generalized Modus Ponens:

Generalized Modus Ponens is a lifted version of Modus Ponens—it raises Modus Ponens Lifting from ground (variable-free) propositional logic to first-order logic. We will see in the rest of this chapter that we can develop lifted versions of the forward chaining, backward chaining, and resolution algorithms introduced in Chapter 7. The key advantage of lifted inference rules over propositionalization is that they make only those substitutions that are required to allow particular inferences to proceed.

### 9.2.1 Unification
Lifted inference rules require finding substitutions that make different logical expressions look identical. This process is called unification and is a key component of all first-order inference algorithms. The UNIFY algorithm takes two sentences and returns a unifier for them (a substitution) if one exists: UNIFY(p,q)=θ where SUBST(θ, p)=SUBST(θ,q).


Let us look at some examples of how UNIFY should behave. Suppose we have a query AskVars(Knows(John, x)): whom does John know? Answers to this query can be found by finding all sentences in the knowledge base that unify with Knows(John, x). Here are the results of unification with four different sentences that might be in the knowledge base:

- UNIFY(Knows(John, x), Knows(John, Jane)) = {x/Jane}
- UNIFY(Knows(John, x), Knows(y,Bill)) = {x/Bill, y/John}
- UNIFY(Knows(John, x), Knows(y,Mother(y))) = {y/John, x/Mother(John)}
- UNIFY(Knows(John, x), Knows(x,Elizabeth)) = failure.
The last unification fails because x cannot take on the values John and Elizabeth at the same time. Now, remember that Knows(x,Elizabeth) means “Everyone knows Elizabeth,” so we should be able to infer that John knows Elizabeth. The problem arises only because the two sentences happen to use the same variable name, x. The problem can be avoided by standardizing apart one of the two sentences being unified, which means renaming its variables to avoid name clashes. For example, we can rename x in Knows(x,Elizabeth) to x17 (a new variable name) without changing its meaning. Now the unification will work:
- UNIFY(Knows(John, x), Knows(x17,Elizabeth)) = {x/Elizabeth, x17/John}.

There is one more complication: we said that UNIFY should return a substitution that makes the two arguments look the same. But there could be more than one such unifier. Every unifiable pair of expressions has a single most general unifier (MGU) that is unique up to renaming and substitution of variables. An algorithm for computing most general unifiers is shown in Figure 9.1.

###  Storage and retrieval
Underlying the TELL, ASK, and ASKVARS functions used to inform and interrogate a knowledge base are the more primitive STORE and FETCH functions. STORE(s) stores a sentence s into the knowledge base and FETCH(q) returns all unifiers such that the query q unifies with some sentence in the knowledge base.

The simplest way to implement STORE and FETCH is to keep all the facts in one long list and unify each query against every element of the list. Such a process is inefficient, but it works. The remainder of this section outlines ways to make retrieval more efficient.

We can make FETCH more efficient by ensuring that unifications are attempted only with sentences that have some chance of unifying. For example, there is no point in trying to unify Knows(John, x) with Brother(Richard, John). We can avoid such unifications by indexing the facts in the knowledge base. A simple scheme called predicate indexing puts all the Knows facts in one bucket and all the Brother facts in another. The buckets can be stored in a hash table for efficient access.

Predicate indexing is useful when there are many predicate symbols but only a few clauses for each symbol. Sometimes, however, a predicate has many clauses. For this particular query, it would help if facts were indexed both by predicate and by second argument, perhaps using a combined hash table key. Given a sentence to be stored, it is possible to construct indices for all possible queries that unify with it. For predicates with a small number of arguments, it is a good tradeoff to create an index for every point in the subsumption lattice. That requires a little more work at storage time, but speeds up retrieval time. However, for a predicate with n arguments, the lattice contains O(2^n) nodes. If function symbols are allowed, the number of nodes is also exponential in the size of the terms in the sentence to be stored.

## 9.3 Forward Chaining

### First-order definite clauses
- Disjunctions of literals with exactly one positive literal
- Form: Antecedent ⇒ Consequent
- No existential quantifiers; universal quantifiers implicit

### Example Problem: Crime
- Represented as first-order definite clauses:
    - "It is a crime for an American to sell weapons to hostile nations"
    - "Nono has some missiles"
    - "All of its missiles were sold to it by Colonel West"
    - Other relevant clauses about hostility, ownership, etc.

### Simple forward-chaining algorithm
- Starting from known facts, trigger rules with satisfied premises
- Add conclusions to known facts until query is answered or no new facts added
- Illustration provided using crime problem

### Efficient forward chaining
- Issues addressed:
    1. Matching rules against known facts
    2. Redundant rule matching
    3. Irrelevant facts
- Incremental forward chaining and the Rete algorithm discussed
- Strategies for efficiency include heuristic matching, incremental updates, and magic sets


## 9.4 Backward Chaining

The second major family of logical inference algorithms uses backward chaining over definite clauses. These algorithms work backward from the goal, chaining through rules to find known facts that support the proof.

###  A backward-chaining algorithm

Figure 9.6 shows a backward-chaining algorithm for definite clauses. `FOL-BC-ASK(KB, goal)` will be proved if the knowledge base contains a rule of the form `lhs ⇒ goal`, where `lhs` (left-hand side) is a list of conjuncts. An atomic fact like `American(West)` is considered as a clause whose `lhs` is the empty list. Now a query that contains variables might be proved in multiple ways. For example, the query `Person(x)` could be proved with the substitution `{x/John}` as well as with `{x/Richard}`. So we implement `FOL-BC-ASK` as a generator—a function that returns multiple times, each time giving one possible result. Backward chaining is a kind of AND/OR search—the OR part because the goal query can be proved by any rule in the knowledge base, and the AND part because all the conjuncts in the `lhs` of a clause must be proved. `FOL-BC-OR` works by fetching all clauses that might function `FOL-BC-ASK(KB, query)` returns a generator of substitutions.

```python
def FOL-BC-ASK(KB, query):
    return FOL-BC-OR(KB, query, {})

def FOL-BC-OR(KB, goal, θ):
    for rule in FETCH-RULES-FOR-GOAL(KB, goal):
        lhs, rhs = STANDARDIZE-VARIABLES(rule)
        for θ0 in FOL-BC-AND(KB, lhs, UNIFY(rhs, goal, θ)):
            yield θ0

def FOL-BC-AND(KB, goals, θ):
    if θ == failure:
        return
    elif LENGTH(goals) == 0:
        yield θ
    else:
        first, rest = FIRST(goals), REST(goals)
        for θ0 in FOL-BC-OR(KB, SUBST(θ, first), θ):
            for θ00 in FOL-BC-AND(KB, rest, θ0):
                yield θ00
```

###  Logic programming
Logic programming is a technology that comes close to embodying the declarative ideal described in Chapter 7: that systems should be constructed by expressing knowledge in a formal language and that problems should be solved by running inference processes on that knowledge. The ideal is summed up in Robert Kowalski’s equation, Algorithm = Logic+Control.

Prolog
Prolog is the most widely used logic programming language. It is used primarily as a rapid-prototyping language and for symbol-manipulation tasks such as writing compilers and parsing natural language. Many expert systems have been written in Prolog for legal, medical, financial, and other domains.

Prolog programs are sets of definite clauses written in a notation somewhat different from standard first-order logic. Prolog uses uppercase letters for variables and lowercase for constants—the opposite of our convention for logic. Commas separate conjuncts in a clause, and the clause is written “backwards” from what we are used to; instead of A∧B ⇒ C in Prolog we have C :- A, B.

## 9.5 Resolution

The last of our three families of logical systems, and the only one that works for any knowledge base, not just definite clauses, is resolution. We saw on page 241 that propositional resolution is a complete inference procedure for propositional logic; in this section, we extend it to first-order logic.

### 9.5.1 Conjunctive normal form for first-order logic

The first step is to convert sentences to conjunctive normal form (CNF)—that is, a conjunction of clauses, where each clause is a disjunction of literals.

In CNF, literals can contain variables, which are assumed to be universally quantified. For example, the sentence

∀x, y,z American(x)∧ Weapon(y)∧Sells(x, y,z)∧Hostile(z) ⇒ Criminal(x)

becomes, in CNF,

¬American(x)∨ ¬Weapon(y)∨ ¬Sells(x, y,z)∨ ¬Hostile(z)∨Criminal(x).

The key is that equivalent CNF sentence. Every sentence of first-order logic can be converted into an inferentially equivalent CNF sentence.

The procedure for conversion to CNF is similar to the propositional case, which we saw on page 244. The principal difference arises from the need to eliminate existential quantifiers.

We illustrate the procedure by translating the sentence “Everyone who loves all animals is loved by someone,” or

∀x [∀y Animal(y) ⇒ Loves(x, y)] ⇒ [∃y Loves(y, x)].

The steps are as follows:

- Eliminate implications: Replace P ⇒ Q with ¬P∨ Q. For our sample sentence, this needs to be done twice:
  - ∀x ¬[∀y Animal(y) ⇒ Loves(x, y)]∨[∃y Loves(y, x)]
  - ∀x ¬[∀y ¬Animal(y)∨Loves(x, y)]∨[∃y Loves(y, x)].
- Move ¬ inwards: In addition to the usual rules for negated connectives, we need rules for negated quantifiers. Thus, we have
  - ¬∀x p becomes ∃x ¬p
  - ¬∃x p becomes ∀x ¬p.
  Our sentence goes through the following transformations:
  - ∀x [∃y ¬(¬Animal(y)∨Loves(x, y))]∨[∃y Loves(y, x)].
  - ∀x [∃y ¬¬Animal(y)∧ ¬Loves(x, y)]∨[∃y Loves(y, x)].
  - ∀x [∃y Animal(y)∧ ¬Loves(x, y)]∨[∃y Loves(y, x)].
  Notice how a universal quantifier (∀y) in the premise of the implication has become an existential quantifier. The sentence now reads “Either there is some animal that x doesn’t love, or (if this is not the case) someone loves x.” Clearly, the meaning of the original sentence has been preserved.
- Standardize variables: For sentences like (∃xP(x)) ∨ (∃xQ(x)) that use the same variable name twice, change the name of one of the variables. This avoids confusion later when we drop the quantifiers. Thus, we have
  - ∀x [∃y Animal(y)∧ ¬Loves(x, y)]∨[∃z Loves(z, x)].
- Skolemize: Skolemization is the process of removing existential quantifiers by elimination. In the simple case, it is just like the Existential Instantiation rule of Section 9.1: translate ∃x P(x) into P(A), where A is a new constant. However, we can’t apply Existential Instantiation to our sentence above because it doesn’t match the pattern ∃v α; only parts of the sentence match the pattern. If we blindly apply the rule to the two matching parts we get
  - ∀x [Animal(A)∧ ¬Loves(x,A)]∨Loves(B, x),
  which has the wrong meaning entirely: it says that everyone either fails to love a particular animal A or is loved by some particular entity B. In fact, our original sentence allows each person to fail to love a different animal or to be loved by a different person. Thus, we want the Skolem entities to depend on x:
  - ∀x [Animal(F(x))∧ ¬Loves(x,F(x))]∨Loves(G(x), x).
  Skolem function Here F and G are Skolem functions. The general rule is that the arguments of the Skolem function are all the universally quantified variables in whose scope the existential quantifier appears. As with Existential Instantiation, the Skolemized sentence is satisfiable exactly when the original sentence is satisfiable.
- Drop universal quantifiers: At this point, all remaining variables must be universally quantified. Therefore, we don’t lose any information if we drop the quantifier:
  - [Animal(F(x))∧ ¬Loves(x,F(x))]∨Loves(G(x), x).
- Distribute ∨ over ∧:
  - [Animal(F(x))∨Loves(G(x), x)]∧[¬Loves(x,F(x))∨Loves(G(x), x)].

This step may also require flattening out nested conjunctions and disjunctions. The sentence is now in CNF and consists of two clauses. It is much more difficult to read than the original sentence with implications. (It may help to explain that the Skolem function F(x) refers to the animal potentially unloved by x, whereas G(x) refers to someone who might love x.) Fortunately, humans seldom need to look at CNF sentences—the translation process is easily automated.

###  The resolution inference rule

The resolution rule for first-order clauses is simply a lifted version of the propositional resolution rule given on page 244. Two clauses, which are assumed to be standardized apart so that they share no variables, can be resolved if they contain complementary literals. Propositional literals are complementary if one is the negation of the other; first-order literals are complementary if one unifies with the negation of the other. Thus, we have

`1 ∨··· ∨`k, m1 ∨··· ∨mn
SUBST(θ, `1 ∨··· ∨`i−1 ∨`i+1 ∨··· ∨`k ∨m1 ∨··· ∨mj−1 ∨mj+1 ∨··· ∨mn)

where UNIFY(`i,¬mj)=θ. For example, we can resolve the two clauses

[Animal(F(x))∨Loves(G(x), x)] and [¬Loves(u, v)∨ ¬Kills(u, v)]

by eliminating the complementary literals Loves(G(x), x) and ¬Loves(u, v), with the unifier θ={u/G(x), v/x}, to produce the resolvent clause

[Animal(F(x))∨ ¬Kills(G(x), x)].

Binary resolution This rule is called the binary resolution

###  Completeness of resolution

This section gives a completeness proof of resolution. It can be safely skipped by those who are willing to take it on faith. We show that resolution is refutation-complete, which means that if a set of sentences is unsatisfiable, then resolution will always be able to derive a contradiction. Resolution cannot be used to generate all logical consequences of a set of sentences, but it can be used to establish that a given sentence is entailed by the set of sentences. Hence, it can be used to find all answers to a given question, Q(x), by proving that KB∧ ¬Q(x) is unsatisfiable.

We take it as given that any sentence in first-order logic (without equality) can be rewritten as a set of clauses in CNF. This can be proved by induction on the form of the sentence, using atomic sentences as the base case (Davis and Putnam, 1960). Our goal therefore is to prove the following: if S is an unsatisfiable set of clauses, then the application of a finite number of resolution steps to S will yield a contradiction.

Our proof sketch follows Robinson’s original proof with some simplifications from Genesereth and Nilsson (1987). The basic structure of the proof (Figure 9.12) is as follows:

1. First, we observe that if S is unsatisfiable, then there exists a particular set of ground instances of the clauses of S such that this set is also unsatisfiable (Herbrand’s theorem).
2. We then appeal to the ground resolution theorem given in Chapter 7, which states that propositional resolution is complete for ground sentences.
3. We then use a lifting lemma to show that, for any propositional resolution proof using the set of ground sentences, there is a corresponding first-order resolution proof using the first-order sentences from which the ground sentences were obtained.

To carry out the first step, we need three new concepts:

- **Herbrand universe**: If S is a set of clauses, then HS, the Herbrand universe of S, is the set of all ground terms constructible from the function symbols and constant symbols in S.
- **Saturation**: If S is a set of clauses and P is a set of ground terms, then P(S), the saturation of S with respect to P, is the set of all ground clauses obtained by applying all possible consistent substitutions of ground terms in P for variables in S.
- **Herbrand base**: The saturation of a set S of clauses with respect to its Herbrand universe is called the Herbrand base of S, written as HS(S).

These definitions allow us to state a form of Herbrand’s theorem (Herbrand, 1930): If a set S of clauses is unsatisfiable, then there exists a finite subset of HS(S) that is also unsatisfiable.

Let S0 be this finite subset of ground sentences. Now, we can appeal to the ground resolution theorem to show that the resolution closure RC(S0) contains the empty clause. That is, running propositional resolution to completion on S0 will derive a contradiction.

Now that we have established that there is always a resolution proof involving some finite subset of the Herbrand base of S, the next step is to show that there is a resolution proof using the clauses of S itself, which are not necessarily ground clauses. We start by considering a single application of the resolution rule. Robinson stated this lemma:

**Lifting Lemma**: Let C1 and C2 be two clauses with no shared variables, and let C01 and C02 be ground instances of C1 and C2. If C0 is a resolvent of C01 and C02, then there exists a clause C such that (1) C is a resolvent of C1 and C2 and (2) C0 is a ground instance of C.

From this fact, it follows that if the empty clause appears in the resolution closure of S0, it must also appear in the resolution closure of S. This is because the empty clause cannot be a ground instance of any other clause. To recap: we have shown that if S is unsatisfiable, then there is a finite derivation of the empty clause using the resolution rule.

The lifting of theorem proving from ground clauses to first-order clauses provides a vast increase in power. This increase comes from the fact that the first-order proof need instantiate variables only as far as necessary for the proof, whereas the ground-clause methods were required to examine a huge number of arbitrary instantiations.

### Equality

None of the inference methods described so far in this chapter can handle an assertion of the form x = y without some additional work. Three distinct approaches can be taken.

1. **Axiomatization of Equality**: Axiomatize equality by writing down sentences about the equality relation in the knowledge base.
2. **Inference Rules for Equality**: Add inference rules rather than axioms. Demodulation and paramodulation are examples of such rules.
3. **Equational Unification**: Handle equality reasoning entirely within an extended unification algorithm.

### Resolution Strategies

We know that repeated applications of the resolution inference rule will eventually find a proof if one exists. In this subsection, we examine strategies that help find proofs efficiently.

- **Unit Preference**: Prefers resolutions where one of the sentences is a single literal (unit clause).
- **Set of Support**: Resolutions must involve at least one element of a special set of clauses—the set of support.
- **Input Resolution**: Resolutions combine one of the input sentences (from the KB or the query) with some other sentence.
- **Linear Resolution**: Allows resolutions between sentences if one is in the original KB or if one is an ancestor of the other in the proof tree.
- **Subsumption**: Eliminates all sentences that are subsumed by an existing sentence in the KB.
- **Learning**: Improve a theorem prover by learning from experience using machine learning techniques.

**Practical Uses of Resolution Theorem Provers**: Resolution theorem provers have been applied in various fields such as hardware design, programming languages, software engineering, and verification.


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

