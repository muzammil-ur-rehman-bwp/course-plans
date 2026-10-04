# Course Plan: Introduction to Artificial Intelligence

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Introduction to Artificial Intelligence |
| Level | Undergraduate (2nd/3rd year, BS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Programming Fundamentals; Data Structures & Algorithms (recommended); basic familiarity with Python syntax is assumed but not taught in this course |
| Programming Language | Python 3.x (used for concept-illustrating labs, not as the course's main subject) |
| Core Libraries | Python standard library (`collections`, `heapq`, `itertools`, `random`); `matplotlib` for simple plots where useful; no NumPy/pandas/scikit-learn dependency except a light, optional touch in the Week 12 ML-survey lab |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab (concept lecture followed by a short hands-on lab) |

## 2. Course Description

This course is a conceptual and theoretical survey of Artificial Intelligence in the tradition of
Russell & Norvig's *Artificial Intelligence: A Modern Approach*. It is **not** an applied
machine-learning-engineering course — it does not build data pipelines or train production
models with libraries such as NumPy, pandas, or scikit-learn. Instead, it builds a principled
understanding of what intelligent behavior is and how it can be computed: how an agent perceives
and acts in an environment, how problems are formulated and solved by search, how knowledge is
represented and reasoned about with logic, how plans are constructed, and how an agent should act
under uncertainty. The course closes with a brief, honest survey of machine learning, neural
networks, natural language processing, computer vision, and robotics — enough for students to see
how the statistical/learning-based pillar of modern AI connects to the classical, symbolic pillar
this course is built around — together with a discussion of AI ethics. Python is used throughout
only as a notation for making abstract algorithms concrete: every lab is a small, self-contained
script that implements or explores a single idea (a search algorithm, a truth-table evaluator, a
minimax game player, a Bayes'-rule calculator), not a data-engineering exercise.

Students who want the applied, library-heavy, "build and train models in Python" treatment of AI
should take *Programming for Artificial Intelligence* instead; this course is deliberately its
conceptual counterpart.

## 3. Goals

- Build a precise vocabulary for describing intelligent agents and their task environments.
- Develop the ability to formulate real-world problems as search, logic, planning, or
  probabilistic-inference problems.
- Understand the classical, symbolic algorithms that defined AI for its first four decades
  (search, logic, planning) deeply enough to trace them by hand and implement them from scratch.
- Understand the basic mathematics of reasoning under uncertainty (probability, Bayes' rule,
  Bayesian networks).
- Gain an accurate, appropriately brief survey of machine learning, neural networks, NLP, computer
  vision, and robotics, and be able to situate each within the broader landscape of AI.
- Think critically about the ethical and societal implications of AI systems.
- Design, build, and present a small AI project applying a classical technique from the course.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Define intelligence, recall AI's historical milestones, and describe a task environment using the PEAS framework. | Remember, Understand |
| CLO2 | Classify agent architectures and environment properties, and explain their implications for agent design. | Understand, Analyze |
| CLO3 | Formulate problems as search problems and implement uninformed, informed, and adversarial search algorithms in Python. | Apply, Analyze |
| CLO4 | Represent knowledge in propositional and first-order logic, and apply inference procedures (truth tables, resolution, forward/backward chaining) to derive conclusions. | Apply, Analyze |
| CLO5 | Construct a simple classical plan (STRIPS-style) for a toy domain, and reason about plan correctness. | Apply, Analyze |
| CLO6 | Apply probability theory and Bayes' rule to reason under uncertainty, and perform basic inference on a small Bayesian network. | Apply, Analyze |
| CLO7 | Summarize, at survey level, the core ideas of machine learning, neural networks, NLP, computer vision, and robotics, and evaluate their ethical implications. | Understand, Evaluate |
| CLO8 | Design, implement, and present an original small-scale AI project built on a classical technique from the course. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

The course is deliberately sequenced to move students up Bloom's cognitive levels:

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundations | 1–2 | Remember, Understand | What AI is; agents and environments |
| Search | 3–5 | Apply, Analyze | Uninformed, informed, and adversarial search |
| Logic | 6–8 | Apply, Analyze | Propositional and first-order logic, inference |
| Planning & Uncertainty | 9–11 | Apply, Analyze | Classical planning, probability, Bayesian networks |
| Survey & Synthesis | 12–16 | Understand, Evaluate, Create | ML/NN/NLP/vision/robotics survey, ethics, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Introduction to AI: history/milestones, definitions of intelligence, the Turing Test, the PEAS framework | Remember, Understand |
| 2 | Intelligent agents: agent types (simple reflex, model-based, goal-based, utility-based), environment properties | Understand, Analyze |
| 3 | Uninformed search: state-space formulation, BFS, DFS, uniform-cost search | Understand, Apply |
| 4 | Informed search: heuristics, admissibility/consistency, A*, local search (hill climbing, simulated annealing) | Apply, Analyze |
| 5 | Adversarial search: minimax, alpha-beta pruning, game-playing (Tic-Tac-Toe) | Apply, Analyze |
| 6 | Knowledge representation & propositional logic: syntax, semantics, truth tables, logical equivalence | Understand, Apply |
| 7 | Propositional logic inference: resolution, forward/backward chaining | Apply, Analyze |
| 8 | First-order logic: syntax, semantics, English-to-FOL translation; midterm review | Understand, Apply |
| 9 | **Midterm Exam** + Classical planning: STRIPS representation, simple planning | Remember–Apply |
| 10 | Uncertainty & probability: joint/conditional probability, Bayes' rule, independence | Understand, Apply |
| 11 | Bayesian networks: representation, basic inference by enumeration | Apply, Analyze |
| 12 | Introduction to Machine Learning (survey): supervised vs. unsupervised, a decision-tree example | Understand, Apply |
| 13 | Introduction to Neural Networks (survey): the perceptron, why deep learning took off | Understand, Apply |
| 14 | Natural Language Processing overview: tokenization, bag-of-words, why language is hard for AI | Understand |
| 15 | Computer Vision & Robotics overview + AI Ethics | Understand, Evaluate |
| 16 | Capstone project presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded scripts/notebooks, submitted weekly (Labs 1–15) |
| Assignments (4) | 20% | Problem sets tied to Weeks 4, 8, 11, 14 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Mini-Project | 20% | Proposal (Wk 11) + implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency.

## 8. Tools & Software

- Python 3.10+ (standard library only for most weeks)
- Jupyter Notebook / Google Colab, or a plain text editor and terminal
- `matplotlib` for the occasional illustrative plot (e.g., Week 13 perceptron demo)
- No NumPy/pandas/scikit-learn dependency is required until the Week 12 ML-survey lab, where a
  tiny, optional use is permitted but not mandatory
- Git/GitHub for lab and project submission

## 9. Reference Textbooks

- Russell, S. & Norvig, P. — *Artificial Intelligence: A Modern Approach* (primary reference;
  the course follows its organization of topics closely: agents, search, logic, planning,
  uncertainty, and the learning/language/vision survey).
- Poole, D. & Mackworth, A. — *Artificial Intelligence: Foundations of Computational Agents*
  (free, open alternative covering the same core material; useful for a second perspective on
  agents, search, and reasoning under uncertainty).
- Official Python documentation (language reference and standard library, for lab support only).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done in
pairs with clearly attributed contributions. Code plagiarism (including uncredited AI-generated
code submitted as original work) is handled per institutional academic integrity policy.
