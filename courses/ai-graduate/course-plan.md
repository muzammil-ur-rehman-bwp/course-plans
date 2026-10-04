# Course Plan: Artificial Intelligence (Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Artificial Intelligence |
| Level | Graduate (MS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab/seminar session of 3 hrs/week) |
| Prerequisites | An undergraduate AI course (e.g., *Introduction to Artificial Intelligence*) or equivalent; comfort with algorithms and asymptotic analysis, basic probability, and Python |
| Programming Language | Python 3.x (used to implement and empirically study every algorithm covered) |
| Core Libraries | Python standard library (`heapq`, `itertools`, `collections`, `random`, `dataclasses`); a SAT-solving library such as `python-sat` (PySAT) is discussed conceptually for Week 6 but no graded component requires it to be installed |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab/Seminar (concept lecture followed by a hands-on implementation or research-skills session) |

## 2. Course Description

This is a rigorous, research-oriented graduate treatment of the core algorithmic and
decision-theoretic foundations of Artificial Intelligence. It assumes students have already
taken a broad undergraduate AI survey (agents, basic search, basic logic, basic planning,
basic uncertainty — as in *Introduction to Artificial Intelligence*) and raises that material to
graduate rigor while extending it into territory the undergraduate survey only touches briefly or
not at all: memory-bounded and bidirectional heuristic search with formal complexity analysis;
game theory and multi-agent systems (normal-form games, Nash equilibrium, mechanism design);
automated reasoning via SAT and SMT solving (DPLL, clause learning, a conceptual look at
satisfiability modulo theories); classical planning studied at the level of its computational
complexity (PSPACE-completeness) together with relaxed-planning-graph heuristics and Hierarchical
Task Network (HTN) planning; Markov Decision Processes and the Bellman equation, value and
policy iteration, Q-learning as a foundational model-free reinforcement-learning algorithm, and
Partially Observable MDPs; probabilistic graphical models studied through the lens of exact vs.
approximate inference and the computational complexity of each; a dedicated unit on the
computational complexity of AI problems (NP-completeness, PSPACE-completeness, and why these
results bound what is practically achievable); and a unit on research methods — how to read and
critique a paper, reproducibility, and sound experimental design — that culminates in a
research-style capstone.

**This course is deliberately scoped to avoid duplicating four sibling graduate courses being
built alongside it.** It does **not** teach neural network architectures or training in depth
(that is *Artificial Neural Network*, Graduate), classical statistical machine learning
algorithms such as SVMs or ensemble methods (that is *Machine Learning*, Graduate), deep learning
architectures such as CNNs/RNNs/Transformers (that is *Deep Learning*, Graduate), or deep
knowledge-representation formalisms such as description logics, frames, and semantic networks
(that is *Knowledge Representation and Reasoning*, Graduate — itself a step up from the
undergraduate KR&R course). Where any of these topics is relevant to an argument in this course
(for example, mentioning that a value function could in principle be approximated by a neural
network), it receives at most a one-sentence pointer to the course that owns it, never a lecture.

## 3. Goals

- Develop graduate-level fluency with advanced search: IDA*, bidirectional search, SMA*, and the
  formal time/space complexity arguments that justify choosing one over another.
- Understand game-theoretic foundations of multi-agent AI: formally correct minimax/alpha-beta
  reasoning, normal-form games, Nash equilibrium, and the basics of mechanism design.
- Understand and implement automated reasoning via SAT solving (DPLL with unit propagation and a
  conceptual treatment of clause learning) and describe how SMT generalizes SAT.
- Analyze classical planning at the level of computational complexity (PSPACE-completeness),
  and use relaxed-planning-graph heuristics and HTN decomposition to make planning tractable.
- Master the MDP formalism, the Bellman equation, value and policy iteration, Q-learning, and the
  exploration-exploitation tradeoff, and extend this understanding to POMDPs via belief states.
- Analyze probabilistic graphical models through the complexity of exact inference and the
  tradeoffs of sampling-based approximate inference.
- Explain, with real complexity-theory results, why SAT/CSP are NP-complete and why planning is
  PSPACE-complete, and what these results imply for the design of practical AI systems.
- Read and critique AI research papers, design small reproducible experiments with proper
  baselines and ablations, and communicate research findings in writing and in a conference-style
  talk.
- Conduct an original, small-scale research project: a literature review, a reproduced or
  extended experiment, a written paper, and a presentation.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the undergraduate AI formalism (agents, search, logic, planning, uncertainty) at graduate rigor and map the research landscape this course covers. | Remember, Understand |
| CLO2 | Implement and formally analyze the time/space complexity of advanced search algorithms (IDA*, bidirectional search, SMA*). | Apply, Analyze |
| CLO3 | Prove the correctness of minimax/alpha-beta search, define and compute Nash equilibria of small normal-form games, and explain how mechanism design aligns incentives in multi-agent systems. | Apply, Analyze, Evaluate |
| CLO4 | Implement the DPLL algorithm for SAT with unit propagation, and explain how SMT solving generalizes SAT to richer theories. | Apply, Analyze |
| CLO5 | Analyze classical planning's computational complexity (PSPACE-completeness), and construct planning-graph heuristics and HTN decompositions that make planning problems tractable in practice. | Apply, Analyze, Evaluate |
| CLO6 | Formulate sequential decision problems as MDPs, derive and apply the Bellman equation, implement value iteration, policy iteration, and Q-learning, and extend the formalism to POMDPs via belief states. | Apply, Analyze, Create |
| CLO7 | Analyze the computational complexity of exact inference in probabilistic graphical models and implement sampling-based approximate inference. | Apply, Analyze |
| CLO8 | Explain, with correct complexity-theoretic statements, why SAT and CSP are NP-complete and why classical planning is PSPACE-complete, and reason about the practical implications. | Understand, Analyze, Evaluate |
| CLO9 | Critique a research paper's claims, methodology, and reproducibility, and design a sound small experiment (baselines, ablations, statistical significance). | Analyze, Evaluate |
| CLO10 | Design, execute, and present an original research-style capstone: a literature review, a reproduced or extended experiment, a written paper, and a conference-style talk. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Graduate Foundations | 1 | Remember, Understand | Research landscape; rigorous agent/problem formalization |
| Advanced Search & Game Theory | 2–4 | Apply, Analyze, Evaluate | IDA*/bidirectional/SMA* search; minimax correctness; Nash equilibrium; multi-agent systems |
| Automated Reasoning & Rigorous Planning | 5–7 | Apply, Analyze | Advanced CSP/metaheuristics; DPLL/SAT/SMT; planning complexity, heuristics, HTN |
| Decision-Theoretic AI | 8–11 | Apply, Analyze, Create | MDPs, Bellman equation, value/policy iteration, Q-learning, POMDPs, graphical-model inference |
| Complexity, Research Methods & Capstone | 12–16 | Analyze, Evaluate, Create | AI-complexity results; paper critique; current trends; capstone research project |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Graduate AI overview: research-areas map, rigorous agent architecture, course expectations (reading/critiquing papers) | Remember, Understand |
| 2 | Advanced search: IDA*, bidirectional search, SMA*, formal time/space complexity analysis | Apply, Analyze |
| 3 | Game theory & adversarial search I: minimax/alpha-beta with a correctness argument; normal-form games; Nash equilibrium | Apply, Analyze |
| 4 | Game theory II & multi-agent systems: cooperative vs. competitive settings; mechanism design and auction theory (brief) | Apply, Analyze, Evaluate |
| 5 | Rigorous CSP & combinatorial optimization: advanced CSP algorithms; metaheuristics (simulated annealing, genetic algorithms) with convergence discussion | Apply, Analyze |
| 6 | Automated reasoning: SAT solving in depth (DPLL, unit propagation, clause learning); SMT overview | Apply, Analyze |
| 7 | Rigorous classical planning: PSPACE-completeness of planning; relaxed-planning-graph heuristics; HTN planning | Apply, Analyze |
| 8 | Markov Decision Processes I: MDP formalism, Bellman equation, value iteration; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + MDPs II: policy iteration, exploration-exploitation, Q-learning | Remember–Apply |
| 10 | Partially Observable MDPs (POMDPs): belief states, worked example | Apply, Analyze |
| 11 | Probabilistic graphical models at rigor: complexity of exact inference; approximate inference by sampling | Apply, Analyze |
| 12 | Computational complexity of AI problems: NP-completeness of SAT/CSP; PSPACE-completeness of planning | Analyze, Evaluate |
| 13 | Research methods in AI: reading/critiquing papers, reproducibility, experimental design | Analyze, Evaluate |
| 14 | Current research topics survey: multi-agent RL, explainable AI (XAI), AI safety/alignment (introductory) | Understand, Evaluate |
| 15 | Research project work session: capstone literature review/experiment guidance, presentation practice | Apply, Create |
| 16 | Capstone research presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 15% | Graded notebooks/scripts, submitted weekly (Labs 1–15) |
| Assignments (3 problem sets) | 15% | Tied to Weeks 4, 8, 12 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-methods assignment; short critique + in-class presentation |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Research Capstone | 25% | Literature review + reproduced/extended experiment + paper + presentation (proposal Wk 7–8, work sessions Wk 15, presentation Wk 16) |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–14 |

**Rationale for the graduate weighting.** Relative to an undergraduate survey course, weight
shifts away from high-stakes closed-book exams (Midterm + Final total 25%, vs. 30% in the
undergraduate course) and toward sustained, research-style work: a dedicated Paper Critique &
Presentation component (10%) and a substantially heavier Research Capstone (25%, vs. 20% for the
undergraduate capstone) that explicitly requires a literature review and an experiment, not just
an implementation. Labs drop slightly (15% vs. 20%) because graduate labs are shorter and more
targeted, assuming stronger baseline programming skill from the prerequisite course.

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (proposal, draft, final submission) have fixed deadlines
because of the downstream presentation schedule; late capstone milestones are handled case-by-case
with the instructor.

## 8. Tools & Software

- Python 3.10+ (standard library for nearly every graded component)
- Jupyter Notebook / Google Colab, or a plain text editor and terminal
- A SAT-solving library such as `python-sat` (PySAT) is introduced conceptually in Week 6 as what
  production SAT solving looks like beyond a teaching-scale DPLL implementation; it is **not**
  required to be installed, and all graded SAT code in this course is a from-scratch Python DPLL
  implementation
- Standard-library-only implementations of MDP/RL examples (grid-world value iteration, policy
  iteration, Q-learning) — no RL framework (e.g., Gymnasium) is required, though students may use
  one for the capstone if their chosen topic calls for it
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks

- Russell, S. & Norvig, P. — *Artificial Intelligence: A Modern Approach* (primary reference;
  this course draws on its advanced chapters — adversarial search and games, constraint
  satisfaction, classical planning, Markov decision processes, and probabilistic reasoning — at
  greater depth and rigor than an undergraduate survey).
- Sutton, R. & Barto, A. — *Reinforcement Learning: An Introduction* (free online; the canonical
  reinforcement-learning textbook, the primary reference for the MDP/Bellman-equation/value-and-
  policy-iteration/Q-learning weeks).
- Current papers from venues such as AAAI, IJCAI, and NeurIPS are assigned as readings from Week
  13 onward (research methods and current-trends weeks); reading and critiquing current
  conference papers is standard practice in a graduate AI course and is treated as a graded skill
  in this course, not an afterthought.
- Official Python documentation (language reference and standard library, for lab support only).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research capstone may be done in
pairs with clearly attributed contributions. Any use of another author's ideas, text, code, or
results — including figures or results from a paper being critiqued or reproduced — must be
properly cited; uncredited reuse of a paper's text or another student's code (including
uncredited AI-generated code or text submitted as original work) is handled per institutional
academic integrity policy. Reproducing a published experiment is expected and encouraged for the
capstone; presenting someone else's reported results as your own experimental findings is not.
