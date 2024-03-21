# Quantifying Uncertainity 
In this chapter, we explore how to manage uncertainty through numeric degrees of belief.

### 12.1 Acting under Uncertainty
Agents operating in the real world must navigate uncertainty arising from partial observability, nondeterminism, or adversarial conditions. They may not know their current state or the outcome of their actions.

Previously, we've seen agents manage uncertainty by tracking a belief state (a set of possible world states they might be in) and generating contingency plans for every potential eventuality reported by their sensors. However, this approach has limitations:
- It requires considering every possible explanation for sensor observations, including highly unlikely ones.
- A correct contingency plan that accounts for every eventuality can become impractically large.
- Sometimes, no plan guarantees success, yet action is necessary. Agents must weigh the merits of plans with uncertain outcomes.

For instance, an automated taxi planning to reach the airport 90 minutes before a flight cannot be certain of success due to various potential disruptions. This scenario illustrates the logical qualification problem, which has no straightforward solution.

The correct approach involves selecting the plan that maximizes the agent's performance measure based on their knowledge of the environment, despite no guarantee of success. Other plans might offer higher certainty but come with their own drawbacks.

###  Summarizing Uncertainty
Uncertainty is common in scenarios like medical diagnoses, where the connection between symptoms and conditions isn't strictly logical. Logic fails to accommodate the complexity of such domains due to:
- Laziness: It's impractical to list all necessary conditions for a rule.
- Theoretical ignorance: Complete domain theories may not exist.
- Practical ignorance: Even with complete knowledge, uncertainty about specific cases remains because not all tests can be performed.

Probability theory emerges as the main tool for dealing with uncertainty, enabling agents to assign numeric degrees of belief to statements. This addresses the qualification problem by summarizing uncertainty from our incomplete knowledge and assumptions.

###  Uncertainty and Rational Decisions
Choosing the best action, such as selecting a plan to get to the airport, requires considering the probabilities of different outcomes and the relative importance of various goals. Decision-making under uncertainty combines preferences (expressed through utility theory) with probabilities to form decision theory, which states that rational agents select actions that yield the highest expected utility.

Utility theory accounts for an agent's preferences for various outcomes, and decision theory integrates these preferences with the likelihood of outcomes to guide rational decision-making.

This chapter lays the groundwork for understanding and computing with probabilistic information, which will be elaborated upon in subsequent chapters covering belief state management, utility theory, and planning in stochastic environments.

## 12.2 Basic Probability Notation

To represent and utilize probabilistic information, we introduce a formal language rooted in probability theory, connecting it to formal logic concepts for AI applications.

###  What Probabilities Are About

Probabilistic assertions, like logical ones, describe possible worlds but focus on the likelihood of these worlds. The *sample space* (Ω) represents all possible worlds, which are mutually exclusive and exhaustive. A probability model assigns a numerical probability (P(ω)) to each possible world, adhering to the axioms that probabilities range between 0 and 1, and their sum equals 1.

Events in probability theory refer to sets of possible worlds, analogous to propositions in logic. The probability of a proposition is the sum of the probabilities of the worlds where it holds.

Unconditional (or prior) probabilities refer to beliefs without additional information. Conditional (or posterior) probabilities, written as P(a|b), consider evidence and represent updated beliefs.

###  The Language of Propositions in Probability Assertions

In probability, variables are known as *random variables*, denoted by uppercase letters, with their possible values defined within a range. Boolean random variables, for example, range between {true, false}. The notation accommodates expressions for both discrete and continuous variables, allowing for probabilities and probability density functions to be defined for a wide range of scenarios.

Probabilities can describe not just single variables but also combinations, leading to concepts like the *joint probability distribution*. This notation facilitates concise expressions of complex probabilistic relationships.

###  Probability Axioms and Their Reasonableness

The foundational axioms of probability theory underpin logical relationships among beliefs about propositions. These include the relationship between a proposition and its negation, as well as the probability of disjunctions, exemplified by the inclusion-exclusion principle.

Kolmogorov's axioms form the basis of probability theory, emphasizing that beliefs must be consistent with these principles to avoid guaranteed losses in a betting framework, as illustrated by Bruno de Finetti's theorem. This notion supports the argument that rational agents' beliefs should adhere to the axioms of probability to ensure coherent decision-making.

Critically, every action or inaction is akin to a bet, with outcomes serving as payoffs. Philosophical and practical arguments advocate for probability theory as the framework for reasoning with degrees of belief, bolstered by the success of probabilistic reasoning systems in various applications.

## 12.3 Inference Using Full Joint Distributions

This section outlines a basic method for probabilistic inference, calculating posterior probabilities for given queries based on observed evidence, utilizing the full joint distribution as the knowledge base.

Consider a domain with three Boolean variables: Toothache, Cavity, and Catch (where a dentist's probe catches in the tooth), represented in a 2x2x2 table. This joint distribution allows for the computation of any proposition's probability by summing the probabilities of the possible worlds where the proposition holds.

### Marginalization

To derive the distribution of a subset of variables, we use *marginalization* (or summing out), adding up probabilities across values of the variables not of interest. This process gives us marginal probabilities. For example, the marginal probability of having a cavity is found by summing probabilities across all possible values of Toothache and Catch where Cavity is true.

### General Marginalization Rule

For any variables Y and a set Z, the marginal probability P(Y) is given by summing over all combinations of Z:

```math
P(Y) = ∑z P(Y,Z=z)
```

Conditioning
Conditioning, derived from the product rule, allows calculating marginal probabilities as well:
```math
P(Y) = ∑z P(Y|z)P(z)
```

These methods are foundational for various probabilistic derivations.

## Calculating Conditional Probabilities
Conditional probabilities, essential for understanding the likelihood of variables given evidence, can be computed from the full joint distribution. For example, the probability of a cavity given a toothache can be calculated directly from the joint distribution table, using the sum of probabilities where both conditions are true divided by the total probability of the evidence (toothache).

Normalization, denoted by α, is often used to ensure the probabilities sum to 1, making calculations more straightforward and allowing progress even without certain probability assessments.

## General Inference Procedure
The general procedure for inference in cases involving discrete variables begins with defining the query variable (X), evidence variables (E), their observed values (e), and any unobserved variables (Y). The query P(X|e) can be computed by:

```math
P(X|e) = α∑y P(X,e,y)
```
where the summation covers all combinations of values for Y, effectively a subset of probabilities from the full joint distribution.

While powerful, this method does not scale well for domains with many variables due to the exponential growth of the full joint distribution table size and computational complexity. Hence, while the full joint distribution forms a theoretical basis for probabilistic reasoning, practical applications require more efficient approaches, laying the groundwork for more advanced systems discussed in subsequent chapters.

## 12.4 Independence

Introducing a fourth variable, Weather, into our full joint distribution example significantly increases the complexity, expanding the table to 32 entries. The relationship between this expanded table and the original is crucial for understanding how we can simplify our calculations.

### Independence and Product Rule

The concept of independence, particularly between events like dental issues and weather conditions, allows us to apply the product rule in a way that simplifies our computations. The assumption that one's dental problems do not affect the weather, and vice versa, lets us assert that the probability of a specific weather condition, given dental issues, is just the probability of that weather condition:

```math
P(cloud |toothache, catch, cavity) = P(cloud).
```

This leads to a simplified equation for combining probabilities:

```math
P(toothache, catch, cavity, cloud) = P(cloud)P(toothache, catch, cavity).

```
Hence, the full joint distribution of Toothache, Catch, Cavity, and Weather can be decomposed into separate distributions for the dental variables and the weather, dramatically reducing the information needed to specify the full joint distribution.

## Expressing Independence
Independence between propositions a and b is expressed as:

```math
P(a|b) = P(a) or P(b|a) = P(b) or P(a∧b) = P(a)P(b).
```

This is equivalent to stating that the variables X and Y are independent if:

```math
P(X |Y) = P(X) or P(Y |X) = P(Y) or P(X,Y) = P(X)P(Y).
```
Independence assertions simplify the representation of the domain and inference processes by reducing the amount of required information. They are based on domain knowledge, like the independence between dental conditions and weather.

## Limitations of Independence
While independence can significantly reduce complexity, clean separations of variables into independent subsets are rare. Connections between variables, no matter how indirect, can negate independence. Moreover, even with independent subsets, the interrelations within these subsets can be complex, necessitating more nuanced methods than outright independence for effective problem-solving.

## 12.5 Bayes’ Rule and Its Use

Bayes' rule is a foundational equation in AI for probabilistic inference, enabling the calculation of posterior probabilities from prior probabilities and likelihoods. It serves as the backbone for modern probabilistic systems.

### Bayes' Rule

The product rule leads to Bayes' rule, which equates two forms of the product rule and allows for the derivation of one conditional probability from others:

```math
P(b|a) = P(a|b)P(b) / P(a)
```

This can be generalized for multivalued variables and background evidence, providing a powerful tool for inference.

## Applying Bayes' Rule: The Simple Case
Bayes' rule is especially useful when we have estimates for P(a|b), P(b), and P(a) and need to compute P(b|a). It turns causal knowledge (e.g., P(effect|cause)) into diagnostic insights (e.g., P(cause|effect)), crucial for areas like medical diagnosis.

For instance, if a doctor knows the probability of symptoms given a disease, Bayes' rule helps to reverse this knowledge to find the probability of the disease given the symptoms, even in light of changing prior probabilities.

## Using Bayes' Rule: Combining Evidence
When dealing with multiple pieces of evidence, Bayes' rule requires adjustments. Conditional independence becomes essential here, allowing for simplification of the calculations. For example, if two symptoms are conditionally independent given a disease, we can calculate the probability of the disease by considering each symptom's effect separately, vastly reducing computational complexity.

Conditional independence of X and Y given Z is defined as:

```math
P(X,Y |Z) = P(X |Z)P(Y |Z)
```

This concept allows for the decomposition of complex joint distributions into simpler components, significantly reducing the information needed for accurate probabilistic modeling.

## Impact of Bayes' Rule and Conditional Independence
Bayes' rule, particularly when combined with conditional independence, enables the scaling of probabilistic systems to handle large sets of variables efficiently. This combination reduces the representation size from exponential to linear growth with the number of symptoms or evidence, facilitating practical probabilistic reasoning in complex domains.

In summary, Bayes' rule and the concept of conditional independence are critical for developing robust and scalable AI systems capable of probabilistic inference in the face of uncertainty.

## 12.6 Naive Bayes Models

Naive Bayes models are a simplification used in many AI systems for probabilistic inference, particularly in cases where a single cause influences several effects, assumed to be conditionally independent given the cause. Despite its simplicity and the conditional independence assumption often being violated in practice, naive Bayes models can be surprisingly effective.

### The Naive Bayes Formula

The full joint distribution in a naive Bayes model is given by:

```math
P(Cause, Effect1, ..., Effectn) = P(Cause) ∏ i P(Effecti | Cause).
```

This formula simplifies the calculation of the probability of the cause given some observed effects, making the computation linear in the number of observed effects without depending on the number of unobserved effects.

## Text Classification with Naive Bayes
Naive Bayes models are particularly useful for text classification tasks, such as categorizing a text document into predefined classes like news, sports, or weather. Here, the "cause" is the document's category, and the "effects" are the presence or absence of certain keywords in the document.

Estimating Probabilities:
- **Prior Probabilities: P(Category)** is estimated based on the fraction of documents in each category observed in the training data.
- **Conditional Probabilities: P(HasWordi | Category)** is estimated based on the fraction of documents in each category that contain a specific keyword.
Classification Process:
To classify a new document, we check for the presence of key words and apply the naive Bayes formula to compute the posterior probability for each category. The document is then classified into the category with the highest posterior probability.

Considerations:
- The naive assumption of independent keyword occurrences is not strictly true, as phrases and related words tend to co-occur more frequently than predicted by independent probabilities. However, the model's predictions, especially the ranking of categories, tend to be quite accurate despite this.
- For critical applications like medical diagnosis, where accurate probability estimates are crucial, more sophisticated models may be preferable.
Naive Bayes models are widely employed for a variety of classification tasks beyond text categorization, such as spam detection, language determination, and document retrieval, demonstrating their versatility and effectiveness despite the simplicity of the underlying assumptions.

## 12.7 The Wumpus World Revisited

Combining concepts from this chapter allows us to tackle probabilistic reasoning problems in the Wumpus World, where an agent's partial sensor information introduces uncertainty. For example, in situations where multiple unvisited squares might contain a pit, a probabilistic agent can evaluate the safety of these squares more effectively than a logical agent, which might have to choose randomly.

### Calculating Probabilities for Pit Locations

The goal is to determine the probability of each square potentially containing a pit, considering that pits cause breezes in neighboring squares and each square (other than [1,1]) has a pit with a probability of 0.2. To model this, we use Boolean variables:

- **Pi,j**: True if square [i, j] contains a pit.
- **Bi,j**: True if square [i, j] is breezy, included only for observed squares.

Given these variables, the full joint distribution is specified, incorporating both the pit configurations and their effects on breezes, alongside their prior probabilities.

### Addressing Computational Complexity

Direct computation from the full joint distribution becomes infeasible with increasing squares due to exponential growth in terms. To mitigate this, we exploit conditional independence: if we knew the pit variables adjacent to the squares of interest, distant pits would not impact our belief about the queried square.

By defining **Frontier** as pit variables adjacent to visited squares and **Other** as remaining unknown squares, we can simplify the computation. Conditional independence allows us to eliminate irrelevant squares, focusing solely on those directly influencing observed breezes.

### Probabilistic vs. Logical Agents in the Wumpus World

A probabilistic agent can assess the safety of squares based on the probability of containing a pit, significantly outperforming a logical agent in decision-making. For instance, calculations might reveal that one square has a much higher likelihood of containing a pit compared to others, guiding the agent to make safer movements.

This approach demonstrates that probability theory, supplemented by independence and conditional independence, provides a robust framework for solving complex reasoning problems efficiently. The next chapter will introduce formal representations and algorithms for exploiting these concepts to conduct probabilistic inference effectively.

# Summary

This chapter has positioned probability theory as a fundamental framework for reasoning under uncertainty, offering an accessible introduction to its application.

- **Uncertainty** is a natural aspect of complex, nondeterministic, or partially observable environments, stemming from both laziness and ignorance. It's unavoidable.
- **Probabilities** articulate the agent's limits in making definitive judgments about the truthfulness of statements. They encapsulate the agent's beliefs in relation to the available evidence.
- **Decision Theory** marries the agent's beliefs and desires, identifying the optimal action as that which maximizes expected utility.
- **Probability Statements** range from prior (unconditional) probabilities to posterior (conditional) probabilities, addressing both simple and compound propositions.
- **Probability Axioms** set bounds on the probabilities of logically connected propositions. Violating these axioms leads to irrational behavior in certain scenarios.
- **The Full Joint Probability Distribution** specifies the probability for every possible assignment of values to variables. Although often too large to be practical in its complete form, it can serve to resolve queries by summing over the relevant possible worlds.
- **Absolute Independence** between variable subsets allows for a decomposition of the full joint distribution into smaller components, significantly simplifying its structure.
- **Bayes’ Rule** facilitates the computation of unknown probabilities from known conditional probabilities, typically from cause to effect. However, incorporating multiple pieces of evidence encounters scalability issues akin to those with the full joint distribution.
- **Conditional Independence**, resulting from direct causal links within the domain, enables the segmentation of the full joint distribution into more manageable, conditional distributions.
- The **Naive Bayes Model** presumes conditional independence among all effect variables given a single cause, scaling linearly with the number of effects. Despite its "naive" assumption, it often performs well in practice.
- In the **Wumpus World**, a probabilistic agent can estimate the likelihood of unobserved elements, enhancing decision-making compared to a solely logical agent. Conditional independence facilitates manageable computations.

This exploration underscores probability theory as a versatile and effective tool for managing uncertainty in AI systems, offering strategies for both conceptual understanding and practical application.
