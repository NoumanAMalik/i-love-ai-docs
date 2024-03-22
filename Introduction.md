# Introduction 

In which we try to explain why we consider artificial intelligence to be a subject most worthy of study, and in which we try to decide what exactly it is, this being a good thing to decide before embarking.

We call ourselves *Homo sapiens*—man the wise—because our intelligence is so important to us. For thousands of years, we have tried to understand how we think and act—that is, how our brain, a mere handful of matter, can perceive, understand, predict, and manipulate a world far larger and more complicated than itself. The field of artificial intelligence, or AI, is concerned with not just understanding but also building intelligent entities—machines that can compute how to act effectively and safely in a wide variety of novel situations.

Surveys regularly rank AI as one of the most interesting and fastest-growing fields, and it is already generating over a trillion dollars a year in revenue. AI expert Kai-Fu Lee predicts that its impact will be “more than anything in the history of mankind.” Moreover, the intellectual frontiers of AI are wide open. Whereas a student of an older science such as physics might feel that the best ideas have already been discovered by Galileo, Newton, Curie, Einstein, and the rest, AI still has many openings for full-time masterminds.

AI currently encompasses a huge variety of subfields, ranging from the general (learning, reasoning, perception, and so on) to the specific, such as playing chess, proving mathematical theorems, writing poetry, driving a car, or diagnosing diseases. AI is relevant to any intellectual task; it is truly a universal field.

<p align="center">
   <img src="https://miro.medium.com/v2/resize:fit:610/1*SJPacPhP4KDEB1AdhOFy_Q.png" alt="Alternate Text" width="350" height="300">
</p>

## 1.1 What Is AI?

We have claimed that AI is interesting, but we have not said what it is. Historically, researchers have pursued several different versions of AI. Some have defined intelligence in terms of fidelity to human performance, while others prefer an abstract, formal definition of intelligence called rationality—loosely speaking, doing the “right thing.” The subject matter itself also varies: some consider intelligence to be a property of internal thought processes and reasoning, while others focus on intelligent behavior, an external characterization.

From these two dimensions—human vs. rational and thought vs. behavior—there are four possible combinations, and there have been adherents and research programs for all four. The methods used are necessarily different: the pursuit of human-like intelligence must be in part an empirical science related to psychology, involving observations and hypotheses about actual human behavior and thought processes; a rationalist approach, on the other hand, involves a combination of mathematics and engineering, and connects to statistics, control theory, and economics. The various groups have both disparaged and helped each other. Let us look at the four approaches in more detail.

###  Acting humanly: The Turing test approach

The Turing test, proposed by Alan Turing (1950), was designed as a thought experiment that would sidestep the philosophical vagueness of the question “Can a machine think?” A computer passes the test if a human interrogator, after posing some written questions, cannot tell whether the written responses come from a person or from a computer.

The computer would need the following capabilities:
- **Natural language processing** to communicate successfully in a human language.
- **Knowledge representation** to store what it knows or hears.
- **Automated reasoning** to answer questions and to draw new conclusions.
- **Machine learning** to adapt to new circumstances and to detect and extrapolate patterns.

Turing viewed the physical simulation of a person as unnecessary to demonstrate intelligence. However, other researchers have proposed a total Turing test, which requires interaction with objects and people in the real world. To pass the total Turing test, a robot will need:
- **Computer vision** and **speech recognition** to perceive the world.
- **Robotics** to manipulate objects and move about.

These six disciplines compose most of AI


<p align="center">
   <img src="https://s.abcnews.com/images/US/TuringTestInfographic_v01_DG_1689800738777_hpEmbed_16x9_992.jpg" alt="Alternate Text" width="500" height="300">
</p>

###  Thinking humanly: The cognitive modeling approach

To say that a program thinks like a human, we must know how humans think. We can learn about human thought in three ways:
- **Introspection:** trying to catch our own thoughts as they go by.
- **Psychological experiments:** observing a person in action.
- **Brain imaging:** observing the brain in action.

Once we have a sufficiently precise theory of the mind, it becomes possible to express the theory as a computer program. If the program’s input–output behavior matches corresponding human behavior, that is evidence that some of the program’s mechanisms could also be operating in humans.

The interdisciplinary field of **cognitive science** brings together computer models from AI and experimental techniques from psychology to construct precise and testable theories of the human mind. Cognitive science is a fascinating field in itself, worthy of several textbooks and at least one encyclopedia. We will occasionally comment on similarities or differences between AI techniques and human cognition.

###  Thinking rationally: The "laws of thought" approach

The Greek philosopher Aristotle was one of the first to attempt to codify "right thinking"—that is, irrefutable reasoning processes. These laws of thought were supposed to govern the operation of the mind; their study initiated the field called logic.

Logicians in the 19th century developed a precise notation for statements about objects in the world and the relations among them. By 1965, programs could, in principle, solve any solvable problem described in logical notation. The so-called **logicist tradition** within artificial intelligence hopes to build on such programs to create intelligent systems.

###  Acting rationally: The rational agent approach

An agent is just something that acts. Computer agents are expected to do more: operate autonomously, perceive their environment, persist over a prolonged time period, adapt to change, and create and pursue goals. A rational agent is one that acts so as to achieve the best outcome or, when there is uncertainty, the best expected outcome.

The rational-agent approach to AI has prevailed throughout most of the field’s history. In the early decades, rational agents were built on logical foundations and formed definite plans to achieve specific goals. Later, methods based on probability theory and machine learning allowed the creation of agents that could make decisions under uncertainty to attain the best expected outcome.

###  Beneficial machines

The standard model has been a useful guide for AI research since its inception, but it is probably not the right model in the long run. The reason is that the standard model assumes that we will supply a fully specified objective to the machine.

The problem of achieving agreement between our true preferences and the objective we put into the machine is called the **value alignment problem**. It becomes more and more difficult to specify the objective completely and correctly as we move into the real world. This leads to the need for a new formulation—one in which the machine is pursuing our objectives but is necessarily uncertain as to what they are.

Ultimately, we want agents that are provably beneficial to humans. This general paradigm is so pervasive that we might call it the standard model. It prevails not only in AI but also in control theory, operations research, statistics, and economics.

## 1.2 The Foundations of Artificial Intelligence

In this section, we explore the multidisciplinary roots of AI, acknowledging the contributions from various fields. It's essential to note that the history highlighted here focuses on a select number of influential figures and ideas, and does not encompass all contributions to AI.

<p align="center">
   <img src="https://www.tutorialspoint.com/artificial_intelligence/images/components_of_ai.jpg" alt="Alternate Text" width="300" height="300">
</p>


###  Philosophy
Philosophy has pondered questions fundamental to AI, such as the possibility of formalizing reasoning, the nature of mind and matter, the origins of knowledge, and how knowledge translates into action. Key contributions include:

- **Aristotle's** formalization of logical reasoning through syllogisms.
- **Ramon Llull** and **Leibniz**'s mechanical attempts to implement reasoning systems.
- **Descartes'** distinction between mind and matter, introducing dualism.
- The development of **empiricism** by **Locke** and **Hume**, emphasizing sensory experience as the source of knowledge.
- **Kant's** deontological ethics, contrasting with **utilitarian** views by **Bentham** and **Mill**, influencing thoughts on rational action.

###  Mathematics
Mathematics provided tools for formal reasoning, computation, and dealing with uncertainty, pivotal for AI's development:

- **Boole** and **Frege**'s foundational work in formal logic.
- The emergence of probability theory, with contributions from **Pascal**, **Fermat**, and **Bayes**.
- **Statistics**' evolution into a field, driven by the analysis of data for inference.
- The conceptualization of algorithms, with **Al-Khwarizmi** and **Euclid** as notable figures.
- **Gödel** and **Turing**'s work on computability, laying the groundwork for understanding what can be computed.

### Economics
Economics introduced the concept of rational decision-making based on preferences, significantly influencing AI:

- **Adam Smith**'s analysis of economic agents' behaviors.
- **Bernoulli** and **Von Neumann**'s work on utility theory and game theory, respectively.
- The development of decision theory and operations research, applying rational choice theory to various decision-making processes.
- **Simon**'s concept of satisficing, highlighting the practical limits of decision-making.

###  Neuroscience
Neuroscience explores how brains process information, contributing insights into AI about cognition and neural processing:

- Early recognition of the brain's significance in thought and consciousness.
- Discoveries in the brain's functional organization, like **Broca**'s area.
- Advances in technology, such as EEG and fMRI, providing deeper understanding of brain activity.
- The development of brain-machine interfaces, illustrating the brain's adaptability.

###  Psychology
Psychology's exploration of thought and behavior has paralleled AI's development, emphasizing cognitive processes:

- **Wundt**'s establishment of experimental psychology.
- The behaviorist movement, led by **Watson**, focusing on observable behavior.
- **Craik** and **Newell & Simon**'s cognitive models, proposing mental representations and processes akin to computational operations.
- The field of cognitive science, integrating AI and psychology to model the mind.

###  Computer Engineering
The evolution of computing technology, from mechanical calculators to modern computers, has been integral to AI:

- The development of programmable machines, like **Babbage**'s Analytical Engine.
- The invention of the digital computer, with pivotal contributions during WWII.
- **Moore's Law** predicting rapid improvements in computing power.
- The rise of specialized AI hardware, like GPUs and TPUs, enhancing machine learning capabilities.

###  Control Theory and Cybernetics
These fields explore how systems can operate under their own control, influencing AI with concepts of feedback and self-regulation:

- Early mechanical systems demonstrating self-regulation.
- **Wiener**'s cybernetics, connecting control systems with cognitive processes.
- The distinction between AI and control theory, despite their shared origins, due to differing mathematical tools and problem domains.

###  Linguistics
Linguistics has deeply influenced AI, particularly in understanding language's relation to thought:

- **Chomsky**'s critique of behaviorist approaches to language, proposing generative grammar.
- The intersection of modern linguistics and AI in computational linguistics, tackling the complexity of language understanding.

This multidisciplinary heritage underscores AI's complexity and its ongoing evolution as a field.

## 1.3 The History of Artificial Intelligence

The history of Artificial Intelligence (AI) can be succinctly summarized by acknowledging the contributions of Turing Award winners. These include Marvin Minsky (1969) and John McCarthy (1971), who laid the foundational bricks of AI based on representation and reasoning. Allen Newell and Herbert Simon (1975) introduced symbolic models for problem-solving and human cognition. Ed Feigenbaum and Raj Reddy (1994) were pivotal in developing expert systems that solve real-world problems by encoding human knowledge. Judea Pearl (2011) revolutionized AI with probabilistic reasoning techniques, addressing uncertainty in a structured manner. The most recent laureates, Yoshua Bengio, Geoffrey Hinton, and Yann LeCun (2019), have been instrumental in integrating deep learning into modern computing, making multilayer neural networks a cornerstone of contemporary AI applications.

###  The Inception of Artificial Intelligence (1943–1956)

The genesis of AI traces back to the work of Warren McCulloch and Walter Pitts in 1943, inspired by Nicolas Rashevsky's mathematical modeling. They proposed a model of artificial neurons, laying the groundwork for computing any function through networks of connected neurons and implementing logical connectives. This era also saw the construction of the first neural network computer, the SNARC, by Marvin Minsky and Dean Edmonds in 1950, symbolizing the nascent steps toward understanding universal computation in neural networks. The period was marked by significant contributions, including Turing's comprehensive vision for AI, articulated in his seminal 1950 paper "Computing Machinery and Intelligence," which introduced concepts such as the Turing test and machine learning. The Dartmouth workshop in 1956, organized by John McCarthy, further catalyzed AI research, albeit without immediate breakthroughs, setting the stage for future developments.

###  Early Enthusiasm, Great Expectations (1952–1969)

This era was characterized by AI's challenge to the skepticism of the 1950s' intellectual establishment with demonstrations of machines performing tasks previously deemed exclusive to humans. The period saw the development of the General Problem Solver (GPS) by Newell and Simon, embodying the "thinking humanly" approach, and Arthur Samuel's checkers-playing programs that leveraged reinforcement learning. John McCarthy's contributions in 1958, including the definition of Lisp and the conceptualization of knowledge-based AI systems, underscored the importance of representation and reasoning in AI's progress.

###  A Dose of Reality (1966–1973)

Despite early optimism, AI researchers soon faced the reality of the field's inherent challenges. Herbert Simon's predictions of AI's imminent success did not materialize as anticipated. The limitations of early AI systems were attributed to a reliance on "informed introspection" and an underestimation of the complexity of the problems being tackled. The era highlighted the computational intractability of many AI problems and marked a period of recalibration in AI research ambitions.

###  Expert Systems (1969–1986)

The development of expert systems marked a significant phase in AI history, focusing on domain-specific knowledge to solve complex problems. The DENDRAL program emerged as an early example, solving molecular structure problems from mass spectrometer data. The period also saw the rise of commercial expert systems, such as the R1 system at Digital Equipment Corporation, demonstrating the practical value of AI in industry. Despite the success, the limitations of expert systems in handling uncertainty and learning from experience eventually led to a reassessment of AI's direction.

###  The Return of Neural Networks (1986–present)

The mid-1980s witnessed a resurgence in neural network research, catalyzed by the reinvention of the back-propagation learning algorithm. This period saw the emergence of connectionist models as a potent alternative to symbolic AI, challenging established norms and paving the way for groundbreaking developments in machine learning and cognitive modeling.

###  Probabilistic Reasoning and Machine Learning (1987–present)

The limitations of expert systems spurred a shift toward a more scientific approach in AI, integrating probabilistic reasoning, machine learning, and experimental validation. This era emphasized the importance of rigorous mathematical foundations and the utility of shared benchmark problem sets for demonstrating progress, marking a mature phase in AI research that seeks to balance theoretical rigor with practical applicability.

The narrative of AI's evolution is one of pioneering ideas, technological breakthroughs, and the continuous quest to understand and replicate the complexities of human intelligence. Each phase of history has contributed layers of understanding, tools, and methodologies that have propelled AI to its current state, reflecting a

# 1.4 The State of the Art

Stanford University's [One Hundred Year Study on AI (AI100)](https://ai100.stanford.edu/) convenes expert panels to report on AI's state of the art. Their 2016 report highlights anticipated increases in AI applications, including self-driving cars, healthcare diagnostics, and elder care, stressing the importance of aligning these advancements with democratic values like freedom, equality, and transparency.

## Highlights from the AI Index Report

- **Publications**: AI research publications have seen a 20-fold increase between 2010 and 2019, with machine learning being the most popular category.
- **Sentiment**: Approximately 70% of AI-related news articles are neutral, though positive sentiment increased from 12% in 2016 to 30% in 2018.
- **Education**: AI course enrollment has surged, indicating a growing interest in the field.
- **Diversity**: The AI field shows a gender disparity, with an 80% male and 20% female composition among professors worldwide.
- **Conferences and Industry Growth**: Notable increases in conference attendance and the number of AI startups reflect the field's rapid expansion.
- **Internationalization**: China leads in the number of published papers, though the U.S. maintains a lead in citation-weighted impact.

## Advances in AI

- **Vision and Language Processing**: Significant improvements in error rates for object detection and language processing models exceed human performance in specific tasks.
- **Speed**: The time required for training AI models on image recognition tasks has dropped significantly.
- **Human Benchmarks**: AI systems have reached or surpassed human-level performance in various domains, including games, language translation, and medical diagnosis.

## Future Projections and Current Capabilities

Experts predict a wide range of timelines for achieving human-level performance across diverse tasks, with opinions varying from as early as 2025 to never. The field's trajectory has shifted from focusing on encoding expert knowledge to leveraging probabilistic models and machine learning techniques without fully understanding the underlying theory.

### Examples of AI Applications Today

- **Autonomous Vehicles**: Demonstrations of self-driving cars and drones highlight significant progress in autonomous navigation.
- **Healthcare**: AI algorithms match or exceed expert performance in diagnosing diseases from medical images.
- **Environmental Science**: AI contributes to climate science by uncovering detailed information about extreme weather events from large datasets.

AI's current state is not just about theoretical advancements but also about practical applications transforming various sectors, emphasizing the blend of science, engineering, and mathematics that underpins these technologies.


# 1.5 Risks and Benefits of AI

Francis Bacon, an influential figure in the development of the scientific method, remarked in *The Wisdom of the Ancients* (1609) on the dual nature of technological advancements, noting their potential to both harm and heal. As AI becomes more ingrained in various aspects of life—economically, socially, scientifically, medically, financially, and militarily—it's crucial to weigh both its potential benefits and risks. The ensuing discussion delves deeper into these aspects, as further elaborated in Chapters 28 and 29.

## Benefits of AI

The crux of AI's potential lies in its ability to elevate the ceiling of human ambitions, thanks to the amplification of our cognitive capabilities through machine intelligence. This could herald an era of unprecedented peace and abundance, marked by liberation from menial tasks and significant boosts in goods and services production. Accelerated scientific research could lead to disease cures and solutions to pressing issues like climate change. As Demis Hassabis of Google DeepMind posits, solving AI could be the key to addressing the multitude of challenges facing humanity.

## Risks Associated with AI

However, the path to "solving AI" is fraught with risks, some already manifesting and others predicted based on current trajectories:

- **Lethal Autonomous Weapons**: These UN-defined weapons, capable of selecting and eliminating targets without human input, pose a scalability risk, potentially allowing small groups to wield disproportionate power.
- **Surveillance and Persuasion**: AI's scalability makes it a potent tool for mass surveillance and social manipulation, as evidenced by recent elections.
- **Biased Decision-Making**: AI applications in parole, loan assessments, and more can perpetuate societal biases if not carefully managed.
- **Employment Impact**: While automation has historically displaced and created jobs, AI's capacity to perform "new kinds of work" raises concerns about future employment landscapes.
- **Safety-Critical Applications**: AI's integration into systems like autonomous vehicles and city infrastructures underscores the importance of rigorous safety standards to prevent fatal accidents.
- **Cybersecurity**: AI enhances both defensive and offensive cyber capabilities, with potential for creating advanced malware for personalized attacks.

## Governance and Regulation

The increase in AI capabilities necessitates thoughtful governance and regulation to mitigate misuse and ensure alignment with societal values. Initiatives by research communities, corporations, and governments aim to establish ethical guidelines and prepare for AI's socio-economic impacts.

## Long-Term Perspectives

The aspiration for human-level or superintelligent AI raises profound questions about control and the ethical implications of surpassing human cognitive abilities. Learning from historical cautionary tales, AI development should prioritize understanding and aligning with human objectives to avoid undesirable outcomes.

## Technical Directions

Despite the prevalence of the "standard model" in AI research, emerging frameworks emphasize machines' alignment with uncertain human objectives, suggesting paths for developing AI systems that genuinely assist humanity.

In conclusion, while AI promises immense benefits, navigating its risks requires careful consideration, ethical commitment, and proactive governance to ensure technology serves the greater good.

