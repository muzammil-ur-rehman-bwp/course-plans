# Course Contents: Introduction to Artificial Intelligence

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Introduction to AI
- **Topics:** What is AI? Four historical views (thinking/acting humanly, thinking/acting
  rationally); brief history and milestones (Dartmouth 1956, expert systems, Deep Blue, AlphaGo,
  the deep learning era); the Turing Test (brief, including common critiques); the PEAS framework
  (Performance measure, Environment, Actuators, Sensors) for describing task environments.
- **Subtopics/Skills:** writing a PEAS description for a given task environment (e.g., an
  automated taxi, a chess player, a vacuum-cleaning robot).
- **Readings:** Russell & Norvig Ch. 1.
- **Software:** Python 3.10+, Jupyter/Colab (environment setup only).

## Week 2 — Intelligent Agents
- **Topics:** Agents and environments; the agent function vs. agent program; agent types —
  simple reflex, model-based reflex, goal-based, utility-based (and a note on learning agents);
  environment properties: fully/partially observable, deterministic/stochastic,
  episodic/sequential, static/dynamic, discrete/continuous, single-agent/multi-agent.
- **Subtopics/Skills:** classifying a given environment by its properties; implementing a simple
  reflex agent and a model-based reflex agent for the vacuum-cleaner world.
- **Readings:** Russell & Norvig Ch. 2.
- **Software:** Python 3.10+.

## Week 3 — Uninformed Search
- **Topics:** Formulating a problem as search (states, actions, transition model, goal test, path
  cost); the search tree vs. graph search distinction; Breadth-First Search (BFS), Depth-First
  Search (DFS), Uniform-Cost Search (UCS).
- **Subtopics/Skills:** implementing a generic `Problem` class and BFS/DFS/UCS solvers in Python
  for a maze/graph problem; comparing completeness, optimality, time/space complexity.
- **Readings:** Russell & Norvig Ch. 3 (§3.1–3.4).
- **Software:** Python, `collections.deque`, `heapq`.

## Week 4 — Informed Search
- **Topics:** Heuristic functions; admissibility and consistency; Greedy Best-First Search; A*
  search and why it is optimal with an admissible heuristic; local search: hill climbing,
  simulated annealing.
- **Subtopics/Skills:** implementing A* with `heapq`; designing and checking a heuristic for a
  grid-path problem; implementing hill climbing and simulated annealing for a toy optimization
  problem.
- **Readings:** Russell & Norvig Ch. 3 (§3.5–3.6), Ch. 4 (§4.1).
- **Software:** Python, `heapq`.
- **Assignment 1 assigned** (agents & search, Weeks 1–4).

## Week 5 — Adversarial Search
- **Topics:** Games as search problems; the minimax algorithm; alpha-beta pruning; evaluation
  functions for non-terminal states (brief); game-playing on Tic-Tac-Toe as a worked example.
- **Subtopics/Skills:** implementing minimax and alpha-beta pruning for Tic-Tac-Toe; measuring
  the reduction in nodes explored from pruning.
- **Readings:** Russell & Norvig Ch. 5 (§5.1–5.3).
- **Software:** Python.

## Week 6 — Knowledge Representation & Propositional Logic
- **Topics:** Why knowledge representation matters; propositional logic syntax (atomic/complex
  sentences, connectives); semantics (models, satisfiability, validity); truth tables; logical
  equivalence; a knowledge base as a set of sentences.
- **Subtopics/Skills:** building a propositional-logic sentence evaluator and truth-table
  generator in Python; checking logical equivalence by comparing truth tables.
- **Readings:** Russell & Norvig Ch. 7 (§7.1–7.4).
- **Software:** Python.

## Week 7 — Propositional Logic Inference
- **Topics:** Entailment; inference by enumeration (model checking); conjunctive normal form
  (CNF); the resolution rule and resolution refutation; Horn clauses; forward chaining and
  backward chaining (brief, conceptual).
- **Subtopics/Skills:** converting sentences to CNF; implementing resolution refutation for a
  small knowledge base; tracing forward chaining on a Horn-clause knowledge base by hand.
- **Readings:** Russell & Norvig Ch. 7 (§7.5).
- **Software:** Python.

## Week 8 — First-Order Logic; Midterm Review
- **Topics:** Limits of propositional logic; first-order logic (FOL) syntax — constants,
  predicates, functions, variables, quantifiers (∀, ∃); semantics (models with objects and
  relations); translating English sentences to FOL; review session for Weeks 1–7.
- **Subtopics/Skills:** translating a set of English statements into FOL and back; practice
  problems for the midterm.
- **Readings:** Russell & Norvig Ch. 8 (§8.1–8.2).
- **Software:** Python (optional: a small well-formedness checker for FOL sentence strings).

## Week 9 — Midterm Exam; Classical Planning
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: classical planning; the STRIPS
  representation (preconditions, add-list, delete-list); a simple planning algorithm (forward
  state-space search over STRIPS actions); a small worked example (toy blocks-world domain).
- **Readings:** Russell & Norvig Ch. 10 (§10.1–10.2).
- **Software:** Python.

## Week 10 — Uncertainty & Probability
- **Topics:** Why logic alone is insufficient under uncertainty; basic probability notation
  (joint, marginal, conditional probability); the product rule; Bayes' rule; conditional
  independence.
- **Subtopics/Skills:** implementing Bayes' rule in Python for a diagnostic-test example
  (computing P(disease | positive test) from sensitivity/specificity/prior prevalence).
- **Readings:** Russell & Norvig Ch. 12 (§12.1–12.5).
- **Software:** Python.
- **Capstone project introduced** (proposal due Week 11).

## Week 11 — Bayesian Networks
- **Topics:** Representing a joint distribution compactly with a Bayesian network (nodes,
  directed edges, conditional probability tables); conditional independence encoded by the graph
  structure; basic inference by enumeration on a small network (hand-worked example).
- **Subtopics/Skills:** building a small Bayesian network in Python (as a dict of conditional
  probability tables) and computing a query probability by enumeration.
- **Readings:** Russell & Norvig Ch. 13 (§13.1–13.3).
- **Software:** Python.
- **Assignment 3 assigned** (planning & probabilistic reasoning, Weeks 9–11). **Capstone proposal
  due.**

## Week 12 — Introduction to Machine Learning (Survey)
- **Topics:** Why learning matters for AI (an agent that improves from experience); supervised
  vs. unsupervised learning; a simple decision-tree example (entropy, information gain, building
  a tiny tree by hand); the train/test idea (conceptual, not a scikit-learn workflow).
- **Subtopics/Skills:** building a tiny decision tree (ID3-style) on a small toy dataset in plain
  Python.
- **Readings:** Russell & Norvig Ch. 19 (§19.1–19.3, overview level).
- **Software:** Python (optional light use of a list/dict-based dataset; no scikit-learn
  required).

## Week 13 — Introduction to Neural Networks (Survey)
- **Topics:** Biological inspiration (brief); the perceptron and the perceptron learning rule;
  linear separability and why a single perceptron cannot learn XOR; why deep (multi-layer)
  networks and more compute/data drove the deep learning resurgence (brief, conceptual, no
  backpropagation derivation).
- **Subtopics/Skills:** implementing a perceptron from scratch in Python and training it on
  AND/OR; demonstrating its failure on XOR.
- **Readings:** Russell & Norvig Ch. 21 (§21.1, overview level).
- **Software:** Python, optional `matplotlib` for a decision-boundary plot.

## Week 14 — Natural Language Processing Overview
- **Topics:** Why natural language is hard for AI (ambiguity, context, world knowledge); basic
  pipeline ideas: tokenization, the bag-of-words representation; a brief look at where modern
  NLP (large language models) fits in the broader AI landscape (survey level only).
- **Subtopics/Skills:** implementing a simple tokenizer and a bag-of-words vectorizer from
  scratch; using word counts for a toy text-classification demo.
- **Readings:** Russell & Norvig Ch. 23 (§23.1, overview level).
- **Software:** Python.
- **Assignment 4 assigned** (learning & language/vision survey, Weeks 12–14).

## Week 15 — Computer Vision & Robotics Overview; AI Ethics
- **Topics:** Computer vision's core problem (recovering structure/meaning from pixel arrays);
  a brief look at edge detection as a simple, concrete vision operation; robotics' core problem
  (sensing, planning, and acting in the physical world); the sense-plan-act cycle; AI ethics —
  bias and fairness, safety, privacy, and societal impact of AI systems.
- **Subtopics/Skills:** a small image-thresholding/edge-detection demo in Python; a simple
  reactive-agent simulation in a grid world; a short ethics case-study discussion.
- **Readings:** Russell & Norvig Ch. 25 (§25.1, overview), Ch. 27 (ethics discussion, overview).
- **Software:** Python (standard library; `matplotlib` optional for displaying a simple image
  array).

## Week 16 — Capstone Presentations; Course Review
- **Topics:** Student capstone project presentations; recap of the course map (agents → search →
  logic → planning → uncertainty → the ML/NN/NLP/vision/robotics survey); closing discussion on
  the relationship between classical, symbolic AI and modern, statistical AI.
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Mini-Project (introduced Week 9, proposal Week 11, final Week 16)
Students (individually or in pairs) pick a small AI problem and apply **one classical technique
from this course** end-to-end: problem formulation → implementation → evaluation/demonstration →
a short written report and a 5–7 minute presentation. The capstone is explicitly **not** a
scikit-learn machine-learning project (that is the applied-programming course's capstone); it
must be built on search, logic, planning, or probabilistic reasoning. Example topics: a
minimax/alpha-beta game-playing agent (e.g., Tic-Tac-Toe, Connect Four, or Nim), a small
logic-based expert system for a narrow domain (e.g., animal identification, simple fault
diagnosis), a toy STRIPS-style planner for an extended blocks-world or logistics domain, or a
Bayesian-network-based diagnostic tool for a small medical or fault-diagnosis scenario.
