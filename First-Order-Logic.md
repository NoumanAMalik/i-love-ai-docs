# First-Order Logic

In which we notice that the world is blessed with many objects, some of which are related to other objects, and in which we endeavor to reason about them. Propositional logic sufficed to illustrate the basic concepts of logic, inference, and knowledge-based agents. Unfortunately, propositional logic is limited in what it can say. In this chapter, we examine first-order logic, which can concisely represent much more. We begin in **Section 8.1** with a discussion of representation languages in general; **Section 8.2** covers the syntax and semantics of first-order logic; **Sections 8.3** and **8.4** illustrate the use of first-order logic for simple representations.

## 8.1 Representation Revisited

In this section, we discuss the nature of representation languages. Programming languages (such as C++ or Java or Python) are the largest class of formal languages in common use. Data structures within programs can be used to represent facts; for example, a program could use a 4 × 4 array to represent the contents of the wumpus world. Thus, the programming language statement `World[2,2]←Pit` is a fairly natural way to assert that there is a pit in square [2,2]. Putting together a string of such statements is sufficient for running a simulation of the wumpus world.

What programming languages lack is a general mechanism for deriving facts from other facts; each update to a data structure is done by a domain-specific procedure whose details are derived by the programmer from his or her own knowledge of the domain. This procedural approach can be contrasted with the declarative nature of propositional logic, in which knowledge and inference are separate, and inference is entirely domain independent. SQL databases take a mix of declarative and procedural knowledge.

A second drawback of data structures in programs (and of databases) is the lack of any easy way to say, for example, “There is a pit in [2,2] or [3,1]” or “If the wumpus is in [1,1] then he is not in [2,2].” Programs can store a single value for each variable, and some systems allow the value to be “unknown,” but they lack the expressiveness required to directly handle partial information.

Propositional logic is a declarative language because its semantics is based on a truth relation between sentences and possible worlds. It also has sufficient expressive power to deal with partial information, using disjunction and negation. Propositional logic has a third property that is desirable in representation languages, namely, compositionality. In a compositional language, the meaning of a sentence is a function of the meaning of its parts. For example, the meaning of “S1,4 ∧ S1,2” is related to the meanings of “S1,4” and “S1,2.” It would be very strange if “S1,4” meant that there is a stench in square [1,4] and “S1,2” meant that there is a stench in square [1,2], but “S1,4 ∧ S1,2” meant that France and Poland drew 1–1 in last week’s ice hockey qualifying match.

However, propositional logic, as a factored representation, lacks the expressive power to concisely describe an environment with many objects. For example, we were forced to write a separate rule about breezes and pits for each square, such as `B1,1 ⇔ (P1,2 ∨ P2,1)`. In English, on the other hand, it seems easy enough to say, once and for all, “Squares adjacent to pits are breezy.” The syntax and semantics of English make it possible to describe the environment concisely: English, like first-order logic, is a structured representation.

### The language of thought

Natural languages (such as English or Spanish) are very expressive indeed. We managed to write almost this whole book in natural language, with only occasional lapses into other languages (mainly mathematics and diagrams). There is a long tradition in linguistics and the philosophy of language that views natural language as a declarative knowledge representation language. If we could uncover the rules for natural language, we could use them in representation and reasoning systems and gain the benefit of the billions of pages that have been written in natural language.

The modern view of natural language is that it serves as a medium for communication rather than pure representation. When a speaker points and says, “Look!” the listener comes to know that, say, Superman has finally appeared over the rooftops. Yet we would not want to say that the sentence “Look!” represents that fact. Rather, the meaning of the sentence depends both on the sentence itself and on the context in which the sentence was spoken. Clearly, one could not store a sentence such as “Look!” in a knowledge base and expect to recover its meaning without also storing a representation of the context—which raises the question of how the context itself can be represented.

Natural languages also suffer from ambiguity, a problem for a representation language. As Pinker (1995) puts it: “When people think about spring, surely they are not confused as to whether they are thinking about a season or something that goes boing—and if one word can correspond to two thoughts, thoughts can’t be words.”

The famous Sapir–Whorf hypothesis (Whorf, 1956) claims that our understanding of the world is strongly influenced by the language we speak. It is certainly true that different speech communities divide up the world differently. The French have two words “chaise” and “fauteuil,” for a concept that English speakers cover with one: “chair.” But English speakers can easily recognize the category fauteuil and give it a name—roughly “open-arm chair”—so does language really make a difference? Whorf relied mainly on intuition and speculation, and his ideas have been largely dismissed, but in the intervening years we actually have real data from anthropological, psychological, and neurological studies.

For example, can you remember which of the following two phrases formed the opening of Section 8.1?
“In this section, we discuss the nature of representation languages ...”
“This section covers the topic of knowledge representation languages ...”
Wanner (1974) did a similar experiment and found that subjects made the right choice at chance level—about 50% of the time—but remembered the content of what they read with better than 90% accuracy. This suggests that people interpret the words they read and form an internal nonverbal representation, and that the exact words are not consequential.

More interesting is the case in which a concept is completely absent in a language. Speakers of the Australian aboriginal language Guugu Yimithirr have no words for relative (or egocentric) directions, such as front, back, right, or left. Instead they use absolute directions, saying, for example, the equivalent of “I have a pain in my north arm.” This difference in language makes a difference in behavior: Guugu Yimithirr speakers are better at navigating in open terrain, while English speakers are better at placing the fork to the right of the plate.

Language also seems to influence thought through seemingly arbitrary grammatical features such as the gender of nouns.


## 8.2 Syntax and Semantics of First-Order Logic

###  Models for first-order logic

Chapter 7 said that the models of a logical language are the formal structures that constitute
the possible worlds under consideration. Each model links the vocabulary of the logical sentences to elements of the possible world, so that the truth of any sentence can be determined.
Thus, models for propositional logic link proposition symbols to predefined truth values.
Models for first-order logic are much more interesting. First, they have objects in them!

**Domain**: The domain of a model is the set of objects or domain elements it contains. The domain is required to be nonempty—every possible world must contain at least one object. Mathematically speaking, it doesn’t matter what these objects are—all that matters is how many there are in each particular model—but for pedagogical purposes, we’ll use a concrete example. [^1]

[^1]: _Note: You might need to include an image here to illustrate the model with five objects._

**Tuple**: Formally speaking, a relation is just the set of tuples of objects that are related. A tuple is a collection of objects arranged in a fixed order and is written with angle brackets surrounding the objects.

**Total functions**: Models in first-order logic require total functions, that is, there must be a value for every input tuple.

###  Symbols and interpretations

The basic syntactic elements of first-order logic are the symbols that stand for objects, relations, and functions.

**Constant symbol**: Symbols that stand for objects.
**Predicate symbol**: Symbols that stand for relations.
**Function symbol**: Symbols that stand for functions.

Each predicate and function symbol comes with an arity that fixes the number of arguments.

###  Terms

A term is a logical expression that refers to an object. Constant symbols are terms, but it is not always convenient to have a distinct symbol to name every object. In the general case, a complex term is formed by a function symbol followed by a parenthesized list of terms as arguments to the function symbol.

### Atomic sentences

Atomic sentences are formed from predicate symbols optionally followed by a parenthesized list of terms.

###  Complex sentences

Logical connectives are used to construct more complex sentences, with the same syntax and semantics as in propositional calculus.

###  Quantifiers

First-order logic contains two standard quantifiers, called universal and existential.

**Universal quantification (∀)**: ...
**Existential quantification (∃)**: ...

###  Equality

First-order logic includes the equality symbol to signify that two terms refer to the same object.

###  Database semantics

Under database semantics, every constant symbol refers to a distinct object, and atomic sentences not known to be true are assumed to be false.

## 8.3 Using First-Order Logic

Now that we have defined an expressive logical language, let’s learn how to use it. In this section, we provide example sentences in some simple domains. In knowledge representation, a domain is just some part of the world about which we wish to express some knowledge. We begin with a brief description of the TELL/ASK interface for first-order knowledge bases. Then we look at the domains of family relationships, numbers, sets, and lists, and at the wumpus world. Section 8.4.2 contains a more substantial example (electronic circuits) and Chapter 10 covers everything in the universe.

### Assertions and queries in first-order logic

Sentences are added to a knowledge base using TELL, exactly as in propositional logic. Such sentences are called assertions. For example, we can assert that John is a king, Richard is a person, and all kings are persons:
- TELL(KB, King(John)).
- TELL(KB, Person(Richard)).
- TELL(KB, ∀x King(x) ⇒ Person(x)).
 
We can ask questions of the knowledge base using ASK. For example, ASK(KB, King(John)) returns true. Questions asked with ASK are called queries or goals. Generally speaking, any query that is logically entailed by the knowledge base should be answered affirmatively. For example, given the three assertions above, the query ASK(KB, Person(John)) 
should also return true. We can ask quantified queries, such as ASK(KB, ∃x Person(x)).
The answer is true, but this is perhaps not as helpful as we would like. It is rather like answering “Can you tell me the time?” with “Yes.” If we want to know what value of x makes the sentence true, we will need a different function, which we call ASKVARS, ASKVARS(KB,Person(x))



and which yields a stream of answers. In this case there will be two answers: {x/John} Substitution and {x/Richard}. Such an answer is called a substitution or binding list. ASKVARS is usually reserved for knowledge bases consisting solely of Horn clauses, because in such knowledge bases every way of making the query true will bind the variables to specific values. That is not the case with first-order logic; in a KB that has been told only that King(John)∨ King(Richard) there is no single binding to x that makes the query ∃x King(x) true, even though the query is in fact true.

### 8.3.2 The kinship domain

The first example we consider is the domain of family relationships, or kinship. This domain includes facts such as “Elizabeth is the mother of Charles” and “Charles is the father of William” and rules such as “One’s grandmother is the mother of one’s parent.”

Clearly, the objects in our domain are people. Unary predicates include Male and Female, among others. Kinship relations—parenthood, brotherhood, marriage, and so on—are represented by binary predicates: Parent, Sibling, Brother, Sister, Child, Daughter, Son, Spouse, Wife, Husband, Grandparent, Grandchild, Cousin, Aunt, and Uncle. We use functions for Mother and Father, because every person has exactly one of each of these, biologically (although we could introduce additional functions for adoptive mothers, surrogate mothers, etc.).

We can go through each function and predicate, writing down what we know in terms of the other symbols. For example, one’s mother is one’s parent who is female:

∀m, c Mother(c)=m ⇔ Female(m)∧Parent(m, c).

One’s husband is one’s male spouse:

∀w,h Husband(h,w) ⇔ Male(h)∧Spouse(h,w).

Parent and child are inverse relations:

∀ p, c Parent(p, c) ⇔ Child(c, p).

A grandparent is a parent of one’s parent:

∀g, c Grandparent(g, c) ⇔ ∃ p Parent(g, p)∧Parent(p, c).

A sibling is another child of one’s parent:

∀x, y Sibling(x, y) ⇔ x 6= y∧ ∃ p Parent(p, x)∧Parent(p, y).

We could go on for several more pages like this, and Exercise 8.KINS asks you to do just that.

Each of these sentences can be viewed as an axiom of the kinship domain, as explained in Section 7.1. Axioms are commonly associated with purely mathematical domains—we will see some axioms for numbers shortly—but they are needed in all domains. They provide the basic factual information from which useful conclusions can be derived. Our kinship axioms are also definitions; they have the form ∀x, y P(x, y) ⇔ .... The axioms define the Mother function and the Husband, Male, Parent, Grandparent, and Sibling predicates in terms of other predicates. Our definitions “bottom out” at a basic set of predicates (Child, Female, etc.) in terms of which the others are ultimately defined.

This is a natural way in which to build up the representation of a domain, and it is analogous to the way in which software packages are built up by successive definitions of subroutines from primitive library functions. Notice that there is not necessarily a unique set of primitive predicates; we could equally well have used Parent instead of Child. In some domains, as we show, there is no clearly identifiable basic set.

Not all logical sentences about a domain are axioms. Some are theorems—that is, they are entailed by the axioms. For example, consider the assertion that siblinghood is symmetric:

∀x, y Sibling(x, y) ⇔ Sibling(y, x).

Is this an axiom or a theorem? In fact, it is a theorem that follows logically from the axiom that defines siblinghood. If we ASK the knowledge base this sentence, it should return true. From a purely logical point of view, a knowledge base need contain only axioms and no theorems, because the theorems do not increase the set of conclusions that follow from the knowledge base. From a practical point of view, theorems are essential to reduce the computational cost of deriving new sentences. Without them, a reasoning system has to start from first principles every time, rather like a physicist having to rederive the rules of calculus for every new problem.

Not all axioms are definitions. Some provide more general information about certain predicates without constituting a definition. Indeed, some predicates have no complete definition because we do not know enough to characterize them fully. For example, there is no obvious definitive way to complete the sentence

∀x Person(x) ⇔ ...

Fortunately, first-order logic allows us to make use of the Person predicate without completely defining it. Instead, we can write partial specifications of properties that every person has and properties that make something a person:

∀x Person(x) ⇒ ...

∀x ... ⇒ Person(x).

Axioms can also be “just plain facts,” such as Male(Jim) and Spouse(Jim,Laura). Such facts form the descriptions.

## 8.4 Knowledge Engineering in First-Order Logic

The preceding section illustrated the use of first-order logic to represent knowledge in three simple domains. This section describes the general process of knowledge-base construction—a process called knowledge engineering. A knowledge engineer is someone who investigates a particular domain, learns what concepts are important in that domain, and creates a formal representation of the objects and relations in the domain. We illustrate the knowledge engineering process in an electronic circuit domain. The approach we take is suitable for developing special-purpose knowledge bases whose domain is carefully circumscribed and whose range of queries is known in advance. General-purpose knowledge bases, which cover a broad range of human knowledge and are intended to support tasks such as natural language understanding, are discussed in Chapter 10.

###  The knowledge engineering process

Knowledge engineering projects vary widely in content, scope, and difficulty, but all such projects include the following steps:

1. **Identify the questions:** The knowledge engineer must delineate the range of questions that the knowledge base will support and the kinds of facts that will be available for each specific problem instance. For example, does the wumpus knowledge base need to be able to choose actions, or is it required only to answer questions about the contents of the environment? Will the sensor facts include the current location? The task will determine what knowledge must be represented in order to connect problem instances to answers. This step is analogous to the PEAS process for designing agents in Chapter 2.

2. **Assemble the relevant knowledge:** The knowledge engineer might already be an expert in the domain or might need to work with real experts to extract what they know—a process called knowledge acquisition. At this stage, the knowledge is not represented formally. The idea is to understand the scope of the knowledge base, as determined by the task, and to understand how the domain actually works. For the wumpus world, which is defined by an artificial set of rules, the relevant knowledge is easy to identify. (Notice, however, that the definition of adjacency was not supplied explicitly in the wumpus-world rules.) For real domains, the issue of relevance can be quite difficult—for example, a system for simulating VLSI designs might or might not need to take into account stray capacitances and skin effects.

3. **Decide on a vocabulary of predicates, functions, and constants:** That is, translate the important domain-level concepts into logic-level names. This involves many questions of knowledge-engineering style. Like programming style, this can have a significant impact on the eventual success of the project. For example, should pits be represented by objects or by a unary predicate on squares? Should the agent’s orientation be a function or a predicate? Should the wumpus’s location depend on time? Once the ontology choices have been made, the result is a vocabulary that is known as the ontology of the domain. The word ontology means a particular theory of the nature of being or existence. The ontology determines what kinds of things exist, but does not determine their specific properties and interrelationships.

4. **Encode general knowledge about the domain:** The knowledge engineer writes down the axioms for all the vocabulary terms. This pins down (to the extent possible) the meaning of the terms, enabling the expert to check the content. Often, this step reveals misconceptions or gaps in the vocabulary that must be fixed by returning to step 3 and iterating through the process.

5. **Encode a description of the problem instance:** If the ontology is well thought out, this step is easy. It involves writing simple atomic sentences about instances of concepts that are already part of the ontology. For a logical agent, problem instances are supplied by the sensors, whereas a “disembodied” knowledge base is given sentences in the same way that traditional programs are given input data.

6. **Pose queries to the inference procedure and get answers:** This is where the reward is: we can let the inference procedure operate on the axioms and problem-specific facts to derive the facts we are interested in knowing. Thus, we avoid the need for writing an application-specific solution algorithm.

7. **Debug and evaluate the knowledge base:** Alas, the answers to queries will seldom be correct on the first try. More precisely, the answers will be correct for the knowledge base as written, assuming that the inference procedure is sound, but they will not be the ones that the user is expecting. For example, if an axiom is missing, some queries will not be answerable from the knowledge base. A considerable debugging process could ensue. Missing axioms or axioms that are too weak can be easily identified by noticing places where the chain of reasoning stops unexpectedly. For example, if the knowledge base includes a diagnostic rule (see Exercise 8.WUMD) for finding the wumpus, ∀s Smelly(s) ⇒ Adjacent(Home(Wumpus),s), instead of the biconditional, then the agent will never be able to prove the absence of wumpuses. Incorrect axioms can be identified because they are false statements about the world. For example, the sentence ∀x NumOfLegs(x,4) ⇒ Mammal(x) is false for reptiles, amphibians, and tables.

To understand this seven-step process better, we now apply it to an extended example—the domain of electronic circuits.

### The electronic circuits domain

We will develop an ontology and knowledge base that allow us to reason about digital circuits of the kind shown in Figure 8.6. We follow the seven-step process for knowledge engineering.

**Identify the questions**
There are many reasoning tasks associated with digital circuits. At the highest level, one analyzes the circuit’s functionality. For example, does the circuit in Figure 8.6 actually add properly? If all the inputs are high, what is the output of gate A2? Questions about the circuit’s structure are also interesting. For example, what are all the gates connected to the first input terminal? Does the circuit contain feedback loops? These will be our tasks in this section. There are more detailed levels of analysis, including those related to timing delays, circuit area, power consumption, production cost, and so on. Each of these levels would require additional knowledge.

**Assemble the relevant knowledge**
What do we know about digital circuits? For our purposes, they are composed of wires and gates. Signals flow along wires to the input terminals of gates, and each gate produces a signal on the output terminal that flows along another wire. To determine what these signals will be, we need to know how the gates transform their input signals. There are four types of gates: AND, OR, and XOR gates have two input terminals, and NOT gates have one. All gates have one output terminal. Circuits, like gates, have input and output terminals. To reason about functionality and connectivity, we do not need to talk about the wires themselves, the paths they take, or the junctions where they come together. All that matters is the connections between terminals—we can say that one output terminal is connected to another input terminal without having to say what actually connects them. Other factors such as the size, shape, color, or cost of the various components are irrelevant to our analysis. If our purpose were something other than verifying designs at the gate level, the ontology would be different. For example, if we were interested in debugging faulty circuits, then it would probably be a good idea to include the wires in the ontology because a faulty wire

## Summary

This chapter has introduced first-order logic, a representation language that is far more powerful than propositional logic. The important points are as follows:

- **Declarative Representation:** Knowledge representation languages should be declarative, compositional, expressive, context independent, and unambiguous.

- **Ontological and Epistemological Commitments:** Logics differ in their ontological commitments and epistemological commitments. While propositional logic commits only to the existence of facts, first-order logic commits to the existence of objects and relations, thereby gaining expressive power appropriate for domains such as the wumpus world and electronic circuits.

- **Limitations in Handling Vagueness:** Both propositional logic and first-order logic share a difficulty in representing vague propositions. This limitation restricts their applicability in domains that require personal judgments, like politics or cuisine.

- **Syntax of First-Order Logic:** The syntax of first-order logic builds on that of propositional logic. It adds terms to represent objects and has universal and existential quantifiers to construct assertions about all or some of the possible values of the quantified variables.

- **Models in First-Order Logic:** A possible world, or model, for first-order logic includes a set of objects and an interpretation that maps constant symbols to objects, predicate symbols to relations among objects, and function symbols to functions on objects. An atomic sentence is true only when the relation named by the predicate holds between the objects named by the terms. Extended interpretations, which map quantifier variables to objects in the model, define the truth of quantified sentences.

- **Developing a Knowledge Base:** Developing a knowledge base in first-order logic requires a careful process of analyzing the domain, choosing a vocabulary, and encoding the axioms required to support the desired inferences.

 








