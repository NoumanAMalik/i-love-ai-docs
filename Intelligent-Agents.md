# Intelligent Agents

In Chapter 2, the nature of agents and their interactions with diverse environments are explored, emphasizing the concept of rational agents which is foundational to artificial intelligence. This chapter aims to apply the notion of rationality to a vast array of agents across any conceivable environment, intending to establish a set of design principles for creating intelligent systems.

## 2.1 Agents and Environments

- **Agents and their Components**: Agents are described as entities perceiving their surroundings via sensors and acting through actuators. An agent's function, which is an abstract mathematical description, is distinguished from the agent program, the concrete implementation running within a physical system.

- **Example - Vacuum-Cleaner World**: A simplified environment, the vacuum-cleaner world, illustrates basic agent functions and program implementation. Agents in this world perceive dirt and can move or clean, demonstrating a basic form of decision-making based on their percept sequence.

The chapter progresses to discuss the good behavior of agents through the lens of rationality, the diverse natures of environments they operate in, and the impact of these environments on the design of effective agents. It provides a foundational framework for understanding intelligent agent behavior and design.

## 2.2 Good Behavior: The Concept of Rationality

A rational agent is one that does the right thing, based on the concept of consequentialism, evaluating behavior by its consequences. Performance measures assess sequences of environment states produced by the agent's actions. Rationality involves choosing actions that maximize the performance measure, considering the agent's knowledge, capabilities, and percepts.

###  Performance Measures

Performance measures are used to define the "right thing" by evaluating sequences of environment states. These measures need to be designed carefully to reflect what is truly desired in the environment rather than how the agent should behave. Challenges include avoiding simple pitfalls and addressing deep philosophical questions about desirability.

###  Rationality

Rationality depends on the performance measure, the agent's prior knowledge, its actions, and its percept sequence. A rational agent chooses actions expected to maximize its performance measure, with considerations for the environment's nature and the agent's capabilities. Rationality is distinguished from omniscience, focusing on expected rather than actual performance.

###  Omniscience, Learning, and Autonomy

Rationality differs from omniscience, as agents cannot predict the actual outcomes of their actions. Rational agents are expected to gather information and learn from their percepts to maximize performance. The degree of an agent's autonomy is determined by its reliance on built-in knowledge versus its learning capabilities.

## 2.3 The Nature of Environments

To build rational agents effectively, understanding the task environments they operate in is crucial, as these environments essentially define the "problems" that agents are designed to solve. Specifying a task environment involves defining the Performance, Environment, Actuators, and Sensors (PEAS) for an agent. This specification guides the design of the agent program to ensure it meets the desired performance measures within its operational environment.

###  Specifying the task environment

The PEAS description is crucial in designing an agent, encompassing the performance measure, the environment itself, and the agent's actuators and sensors. For instance, an automated taxi driver's task environment would consider safety, legal compliance, comfort, profitability, and minimal disruption to others.

### Properties of task environments

Task environments vary widely, but can be categorized along several dimensions such as:

- **Fully vs. Partially Observable**: Whether an agent's sensors can access the complete state of the environment.
- **Single-agent vs. Multiagent**: Whether the environment involves other agents that might affect the agent's performance.
- **Deterministic vs. Nondeterministic**: If the next state of the environment is completely determined by current actions.
- **Episodic vs. Sequential**: Whether the agent's experience is divided into atomic episodes or if current decisions affect future ones.
- **Static vs. Dynamic**: Whether the environment changes while the agent is deliberating.
- **Discrete vs. Continuous**: The nature of the environment, time, percepts, and actions.
- **Known vs. Unknown**: If the agent has knowledge about the environment's "laws of physics".

Understanding these properties helps in designing suitable agents for specific environments, ensuring they behave rationally according to the defined performance measures.

## 2.4 The Structure of Agents

In exploring artificial intelligence (AI), understanding the internal workings of agents is crucial. Agents perform actions based on percepts. Designing an effective agent involves creating an agent program that accurately maps these percepts to actions, termed as the *Agent Function*. This program operates within a computing device equipped with sensors and actuators, known as the *Agent Architecture*.

### Agent Architecture
- **Definition**: A combination of hardware (architecture) and software (program) that enables an agent to perceive its environment and act upon it.
- **Examples**: Ranges from PCs to robotic cars equipped with cameras and sensors.

---
### Agent Programs

### Table-Driven Agents
- **Description**: Agents that operate based on a pre-defined table of actions corresponding to sequences of percepts.
- **Limitations**: Impractical due to the exponential growth of the table with the variety of percepts and actions.

### Types of Agent Programs
1. **Simple Reflex Agents**: Act on the current percept, ignoring history.
2. **Model-Based Reflex Agents**: Maintain an internal state to reflect unseen aspects of the current state.
3. **Goal-Based Agents**: Actions are taken based on goals and the current state.
4. **Utility-Based Agents**: Actions are chosen based on a utility function, aiming to maximize happiness or satisfaction.

### Learning Agents
Agents that adapt and improve over time through learning mechanisms, divided into:
- **Performance Element**: Decides actions based on percepts.
- **Learning Element**: Improves the performance element based on feedback.
- **Critic**: Provides feedback to the learning element based on a performance standard.
- **Problem Generator**: Suggests exploratory actions to enhance learning.

### Representation in Agent Programs
Understanding the environment requires various levels of representation:
- **Atomic Representation**: Indivisible states with no internal structure.
- **Factored Representation**: States represented by a set of variables or attributes.
- **Structured Representation**: States include objects, attributes, and their relationships.

#### Expressiveness
- The complexity and expressive power of representations range from atomic, factored, to structured, with increasing ability to capture detailed and nuanced information about the environment.

#### Localist vs. Distributed Representation
- **Localist Representation**: One-to-one mapping between concepts and memory locations.
- **Distributed Representation**: A concept is represented across multiple memory locations, enhancing robustness against noise and information loss.

