# Knowledge Representation

## Introduction
Knowledge Representation involves detailing how to encode the diverse facts about the real world into a form conducive to reasoning and problem-solving. This chapter focuses on the construction of an agent's knowledge base using first-order logic as the primary language, though later discussions will explore other formalisms for specific reasoning tasks across various domains.

## 10.1 Ontological Engineering
Ontological engineering is the process of organizing the world's entities into a structured hierarchy or ontology, enabling general and flexible representations for complex domains like internet shopping or navigating traffic. This section introduces the concept of a general ontology that categorizes everything in the world, providing a foundation for more domain-specific knowledge to be incorporated.

### Upper Ontology
The upper ontology represents a high-level categorization of concepts, ranging from abstract objects to physical objects, and serves as a scaffolding where domain-specific details can be added. The importance of ontological engineering lies in its ability to provide a general-purpose framework applicable across various domains, demanding a unified approach to representing diverse knowledge areas.

## 10.2 Categories and Objects
Categories organize objects, allowing reasoning at the level of categories rather than individual instances. This organization is crucial for making predictions and inferences about objects based on their category membership.

### Physical Composition
Objects can be part of other objects, creating hierarchies of parts within composite objects. These structural relationships among parts define composite objects, such as defining a biped based on its body and legs.

### Measurements
Objects possess measures like height, mass, and cost. While quantitative measures are straightforward to represent, qualitative measures such as beauty or difficulty require ordered relations rather than numerical values, facilitating reasoning without exact numerical scales.

### Objects: Things and Stuff
The distinction between things (count nouns) and stuff (mass nouns) reflects how we interact with and categorize objects. Stuff, like butter or water, defies easy individuation into distinct objects, leading to different representational strategies for handling both things and stuff in knowledge bases.

## Ontologies from Various Routes
The creation of ontologies can follow different paths, from expertly crafted axioms to extracting facts from existing databases or text documents, and even crowdsourcing commonsense knowledge. This diversity in creation methods reflects the challenges and possibilities in developing ontologies that effectively capture the world's complexity.

## Challenges in Ontological Engineering
Despite its potential, general ontological engineering faces significant challenges, including social and political factors that complicate the agreement on a common ontology. Specialized ontologies have seen more success, focusing on domain-specific knowledge engineering and machine learning.

Knowledge Representation and Ontological Engineering form the basis for encoding real-world facts into a computationally usable form. Through the use of ontologies and careful consideration of categories, objects, and their properties, it is possible to build rich knowledge bases that support effective reasoning and problem-solving across diverse domains.


## 10.3 Events

The event calculus is introduced to handle a broader range of actions or events beyond the capabilities of successor-state axioms, accommodating continuous actions, simultaneous events, and other complexities. This approach enriches the representation of events, fluents, and time points, offering a more nuanced understanding of actions and their effects over time.

###  Time

Event calculus facilitates discussions about time points and intervals, with distinctions between moments and extended intervals. A set of predicates is defined to describe relationships between time intervals, enabling more sophisticated temporal reasoning.

### Fluents and Objects

Physical objects can be viewed as generalized events, with properties changing over time. The concept of fluents is extended to objects, illustrating how objects like the presidency of the USA can be represented as changing entities over time.

This chapter lays the groundwork for representing and reasoning about a wide range of real-world phenomena, emphasizing the importance of ontological engineering and the challenges involved in capturing the complexity of the real world.

## 10.4 Mental Objects and Modal Logic

The agents we have constructed so far have beliefs and can deduce new beliefs. Yet, none of them has any knowledge about beliefs or about deduction. Knowledge about one’s own knowledge and reasoning processes is crucial for controlling inference. For example, if Alice asks, "what is the square root of 1764," and Bob replies, "I don’t know," but upon insistence to "think harder," Bob should realize that the question can, in fact, be answered through further thought. Conversely, if the question were, "Is the president sitting down right now?" Bob should recognize that thinking harder is unlikely to aid in answering this question. Knowledge about the knowledge of other agents is also significant; for instance, Bob should understand that the president would know their own sitting status.

To address these scenarios, we need a model of the mental objects and processes within an individual’s or entity’s knowledge base. This model doesn’t need to be detailed down to prediction times for deductions but should enable conclusions such as a mother knowing her sitting status.

### Propositional Attitudes

Agents can hold various propositional attitudes towards mental objects, such as beliefs, knowledge, desires, and intentions. These attitudes, however, do not function as "normal" predicates. For example, asserting that Lois knows Superman can fly presents issues since, under normal logic that includes equality reasoning, if Superman is indeed Clark Kent, then it erroneously follows that Lois knows Clark can fly—contradicting the typical narrative that Lois does not know Clark is Superman.

### Referential Transparency vs. Opacity

Normal logic operates under referential transparency, where the terms used to refer to an object are interchangeable without affecting the truth of statements. However, for propositional attitudes, we desire referential opacity, where the specific terms used do matter, as agents may not know the equivalence of different terms referring to the same entity.

### Modal Logic

Modal logic addresses these challenges by introducing modal operators that apply to sentences, allowing expressions like "A knows P." Unlike first-order logic that models a singular true world, modal logic employs a collection of possible worlds, enhancing the ability to model knowledge and beliefs accurately. Each world is interconnected through accessibility relations, representing what is known or possible from the perspective of various agents.

#### Knowledge and Modal Operators

In modal logic, a statement like "agent A knows P" is true in a given world if P is true in all worlds accessible from the perspective of A. This framework also supports nested knowledge, enabling the modeling of what one agent knows about another’s knowledge or beliefs.

### Challenges and Extensions

While modal logic significantly advances the modeling of knowledge and beliefs, it assumes logical omniscience among agents—that if an agent knows a set of axioms, it consequently knows all derivable conclusions. This assumption is often unrealistic, leading to explorations of bounded rationality, where agents' knowledge or beliefs are limited by the complexity or duration of reasoning processes.

#### Other Modal Logics

Beyond knowledge, various modal logics have been proposed to capture other modalities, such as possibility, necessity, and temporal dynamics. These extensions provide richer frameworks for modeling the multifaceted nature of agents' mental states and the complexities of their interactions with the world and each other.

In conclusion, modal logic offers a powerful tool for representing and reasoning about the mental states of agents, addressing the limitations of traditional logic in handling propositional attitudes and the nuances of knowledge, belief, and inference.


## 10.5 Reasoning Systems for Categories

Categories are essential for organizing and reasoning within large-scale knowledge representation schemes. There are two closely related system families for this purpose: **semantic networks** and **description logics**. Semantic networks aid in visualizing knowledge bases and inferring object properties based on category membership. Description logics, on the other hand, offer a formal language for defining and combining categories and algorithms for deciding subset and superset relationships between categories.

###  Semantic Networks

Originating from Charles S. Peirce's existential graphs in 1909, semantic networks are essentially a graphical form of logic. They represent objects, categories, and the relationships between them using nodes and edges. Despite the simplicity and utility of semantic networks, a debate between their proponents and advocates of traditional "logic" obscured their underlying logical nature.

Semantic networks facilitate inheritance reasoning, allowing objects to inherit properties from the categories to which they belong. Multiple inheritance, however, introduces complexities when an object belongs to more than one category or a category is a subset of multiple others. Some programming languages restrict multiple inheritance due to these complexities.

One limitation of semantic networks is their focus on binary relations, making the representation of n-ary relations less direct. Yet, through reification—representing propositions as events belonging to specific categories—semantic networks can emulate n-ary relations.

### Description Logics

Description logics evolved from semantic networks, formalizing their concepts while retaining an emphasis on taxonomic structure. The key operations in description logics are subsumption (determining if one category is a subset of another) and classification (determining category membership). Description logics aim to make it easier to define and work with categories, as opposed to first-order logic, which is more object-oriented.

A notable example is the CLASSIC language, which uses a combination of operations on predicates to express complex category definitions. Description logics prioritize tractability of inference, ensuring that subsumption testing can be solved within polynomial time. This emphasis on efficiency helps avoid the unpredictably long solution times associated with standard first-order logic systems. However, the limitation to tractable constructs means that either hard problems cannot be stated or require exponentially large descriptions.

### Key Differences and Contributions

Both semantic networks and description logics contribute significantly to knowledge representation, each with its unique strengths. Semantic networks offer a visually intuitive way to model knowledge bases, simplifying the inference process through inheritance reasoning. Description logics provide a formal framework for defining categories and their relationships, focusing on the efficiency and tractability of inference operations. Together, these systems form the backbone of category-based reasoning in large-scale knowledge representation schemes.

## 10.6 Reasoning with Default Information

The complexity of real-world reasoning often involves making assumptions or accepting statements as generally true until proven otherwise. This section explores the concepts and mechanisms behind reasoning with default information, providing insights into the nonmonotonicity of logic and the frameworks developed to manage defaults: circumscription, default logic, and truth maintenance systems.

###  Circumscription and Default Logic

Monotonicity, a property of classical logic, states that the addition of new information to a knowledge base (KB) does not invalidate existing conclusions. However, real-world reasoning often requires nonmonotonic approaches, where conclusions can be revised or retracted in light of new evidence. Two significant nonmonotonic logics are circumscription and default logic.

#### Circumscription

Circumscription aims to minimize the extension of certain predicates, assuming they are false unless explicitly stated as true. This approach enables reasoning with defaults by specifying exceptions to general rules. For instance, the assertion that "birds typically fly" can be modeled by introducing an `Abnormal` predicate to denote exceptions. Circumscription then allows for reasoning about whether specific instances (e.g., Tweety the bird) adhere to the default rule or the exception, based on the available information.

#### Default Logic

Default logic offers a way to express rules that apply in the absence of contradictory information. A default rule has the form `P : J/C`, where `P` is a prerequisite, `J` are justifications, and `C` is a conclusion. If `P` holds and `J` is consistent with the current knowledge, then `C` can be inferred. This framework can model complex scenarios, such as the "Nixon diamond," where conflicting defaults about Nixon being a pacifist or not are resolved through the consideration of multiple extensions of a default theory.

###  Truth Maintenance Systems (TMS)

As reasoning with default information often leads to conclusions that may later need to be revised, Truth Maintenance Systems (TMS) are designed to manage this dynamic nature of knowledge. They track the justifications for beliefs, enabling efficient belief revision and retraction when new evidence contradicts previous inferences.

#### Types of TMS

- **Justification-based TMS (JTMS)**: Every sentence in the KB is annotated with its justifications, allowing for efficient management of dependencies and facilitating the retraction of inferences when their bases are no longer valid.
- **Assumption-based TMS (ATMS)**: An ATMS keeps track of the assumptions underlying each conclusion, representing multiple hypothetical worlds simultaneously. This allows for rapid context switching and exploration of different scenarios without re-computation.

#### Applications and Implications

TMSs not only support the retraction of incorrect inferences but also enhance the system's capability to handle complex environments and hypothetical reasoning. They offer mechanisms for explaining conclusions through the tracing of assumptions and justifications, underpinning the adaptive and responsive nature of intelligent reasoning systems.

In summary, reasoning with default information requires sophisticated logical frameworks and systems capable of handling nonmonotonicity and belief revision. Circumscription, default logic, and TMSs represent crucial advancements in the development of flexible, real-world reasoning systems.


# Summary of Knowledge Representation

This summary encapsulates the essence of knowledge representation, highlighting the construction of real-world knowledge bases and the philosophical challenges encountered. The main takeaways are:

- **General-purpose Ontology**: Essential for large-scale knowledge representation, a general-purpose ontology integrates various specific knowledge domains, aiming to universally cover a wide array of knowledge. Constructing such an ontology poses a substantial challenge, with current frameworks showing robust potential but not yet fully realized.

- **Upper Ontology**: The discussed upper ontology, grounded in categories and the event calculus, spans across categories, subcategories, parts, structured objects, measurements, substances, events, time, space, change, and beliefs. This comprehensive structure serves as the foundation for representing a wide variety of knowledge domains.

- **Natural Kinds and Event Calculus**: While natural kinds resist complete logical definition, their properties can be effectively represented. The event calculus facilitates the representation of actions, events, and temporal sequences, enabling logical deductions about the outcomes of actions.

- **Special-purpose Representation Systems**: Systems like semantic networks and description logics are tailored for categorizing knowledge into a hierarchical structure. Inheritance mechanisms within these systems permit the deduction of object properties based on category membership.

- **Closed-world Assumption and Nonmonotonic Logics**: The closed-world assumption simplifies knowledge representation by negating the need for extensive negative information, acting as a default mechanism that can be overridden. Nonmonotonic logics, including circumscription and default logic, aim to encompass general default reasoning practices.

- **Truth Maintenance Systems (TMS)**: These systems are designed to efficiently manage updates and revisions within knowledge bases, ensuring that knowledge remains current and accurate despite the dynamic nature of real-world information.

- **Ontology Construction Challenges**: Manual construction of large ontologies is daunting. Leveraging text extraction techniques can significantly ease the process, although challenges remain in accurately capturing and organizing complex knowledge structures.

This overview serves to underline the complexity and necessity of sophisticated knowledge representation systems in simulating intelligent reasoning and understanding within artificial agents.




