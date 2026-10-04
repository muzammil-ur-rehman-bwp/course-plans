# Course Plan: Knowledge Representation and Reasoning

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Knowledge Representation and Reasoning |
| Level | Undergraduate (2nd/3rd year, BS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Programming Fundamentals (required); discrete mathematics / propositional & predicate logic (recommended) |
| Programming Language | Python 3.x (used to implement every representation and inference algorithm covered) |
| Core Libraries | Python standard library only (`itertools`, `collections`, `functools`, `dataclasses`); no external KR-specific tooling is required to pass the course |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab (concept lecture followed by a hands-on implementation lab) |

## 2. Course Description

*Introduction to Artificial Intelligence* gives students a brief, broad survey of classical AI in
which propositional/first-order logic, STRIPS planning, and Bayesian networks each receive a
single week as one stop among many (search, planning, uncertainty, and a machine-learning
survey). **This course is different: it is a full semester dedicated entirely to Knowledge
Representation and Reasoning (KR&R)**, and it goes considerably deeper and broader than that
survey. Where the introductory course asks "how does an AI agent work, in broad strokes?", this
course asks "how, precisely, is knowledge formally represented inside a reasoning system, and
how does that system correctly derive new conclusions from it?"

The course covers propositional and first-order logic in genuine depth (normal forms, resolution
refutation, unification, Skolemization); structured representation schemes that the introductory
survey never touches at all — semantic networks, frames with inheritance and defaults,
description logics and ontologies (with an honest look at OWL/RDF and the Semantic Web);
rule-based production systems built from scratch; constraint satisfaction studied as a reasoning
formalism in its own right (arc consistency, backtracking heuristics); non-monotonic reasoning
(the closed-world assumption, default logic, circumscription) — reasoning that can retract
conclusions as new information arrives, which classical logic cannot do; classical planning
studied in genuine depth (STRIPS, partial-order planning, planning graphs) well beyond the
one-week forward-search treatment given elsewhere; temporal and spatial reasoning (Allen's
interval algebra); probabilistic reasoning and Bayesian networks with exact inference by
variable elimination (a level deeper than a Bayes'-rule-only treatment); and a survey of
reasoning under uncertainty beyond Bayes (Markov logic networks, fuzzy logic). The course closes
by building an integrated knowledge-based reasoning agent and surveying current trends —
knowledge graphs and neuro-symbolic AI — and how KR systems are evaluated.

Every formalism is paired with a correct, from-scratch Python implementation of its core
algorithm: a resolution-refutation prover, a unification function, a frame-inheritance resolver,
an AC-3 arc-consistency filter, a default-logic extension builder, a planning-graph builder, an
Allen's-algebra constraint propagator, and a Bayesian-network variable-elimination engine. Real
KR tools that the field actually uses — Prolog as a logic-programming language, and
description-logic/ontology reasoners such as Pellet or editors such as Protégé for OWL — are
discussed as real-world context so students know what production KR tooling looks like, but all
graded code in this course is plain Python.

Students who want the broad classical-AI survey (search, a single week of logic, a single week
of planning, a single week of Bayesian networks, alongside an ML/NN/NLP/vision survey) should
take *Introduction to Artificial Intelligence* instead; this course is its deep, KR-focused
counterpart.

## 3. Goals

- Master propositional and first-order logic as formal representation languages, including the
  syntax/semantics distinction, normal forms, and sound and complete inference procedures.
- Implement, from scratch, the core algorithms of symbolic reasoning: resolution refutation,
  unification, forward/backward chaining, arc consistency, backtracking search, and Bayesian
  variable elimination.
- Compare and choose among representation formalisms (logic, semantic networks, frames,
  production rules, description logics, constraint networks) based on their expressiveness,
  inferential efficiency, and naturalness for a given domain.
- Understand why classical (monotonic) logic is insufficient for everyday common-sense reasoning,
  and apply non-monotonic reasoning techniques that can retract conclusions.
- Represent and reason about time and (at an introductory level) space.
- Apply probabilistic graphical models and alternative uncertainty formalisms (fuzzy logic,
  Markov logic networks) where logic alone is insufficient.
- Design, build, and present a capstone knowledge-based reasoning system that integrates at least
  two distinct formalisms from the course.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Explain the desiderata for a good knowledge representation and map the landscape of representation schemes covered in the course. | Remember, Understand |
| CLO2 | Represent domain knowledge in propositional and first-order logic, convert sentences to normal forms, and apply resolution refutation (propositional and first-order, with unification) to derive conclusions. | Apply, Analyze |
| CLO3 | Build rule-based production systems (forward- and backward-chaining) and structured representations (semantic networks, frames with inheritance and defaults) from scratch in Python. | Apply, Create |
| CLO4 | Explain description logics and their relationship to first-order logic, and describe how OWL/RDF realize the Semantic Web vision. | Understand, Analyze |
| CLO5 | Formulate problems as constraint satisfaction problems and apply arc consistency (AC-3) and heuristic backtracking search to solve them. | Apply, Analyze |
| CLO6 | Contrast monotonic and non-monotonic reasoning, and apply the closed-world assumption and default logic to derive and correctly retract conclusions. | Understand, Apply, Analyze |
| CLO7 | Construct STRIPS-based plans in depth (including partial-order planning and planning graphs) and reason about time using Allen's interval algebra. | Apply, Analyze |
| CLO8 | Perform exact probabilistic inference on a Bayesian network by variable elimination, and describe alternative uncertainty formalisms (Markov logic networks, fuzzy logic). | Apply, Analyze, Understand |
| CLO9 | Design, implement, and present a capstone knowledge-based reasoning system integrating at least two formalisms from the course, and evaluate its correctness and limitations. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Logic Foundations | 1–4 | Understand, Apply | KR desiderata; propositional and first-order logic in depth; unification and FOL inference |
| Representation Schemes | 5–7 | Apply, Create | Rule-based systems; semantic networks and frames; description logics and ontologies |
| Constraints, Non-Monotonic Reasoning & Planning | 8–10 | Apply, Analyze | CSP algorithms; midterm; non-monotonic reasoning; planning in depth |
| Temporal/Probabilistic Reasoning & Trends | 11–15 | Apply, Analyze, Evaluate | Temporal/spatial reasoning; Bayesian networks; uncertainty beyond Bayes; KB agents; current trends |
| Synthesis | 16 | Evaluate, Create | Capstone presentations; course review |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Introduction to Knowledge Representation: desiderata, map of representation schemes | Remember, Understand |
| 2 | Propositional logic in depth: normal forms, resolution refutation, SAT and complexity | Understand, Apply |
| 3 | First-order logic in depth: syntax, semantics, English-to-FOL translation | Understand, Apply |
| 4 | First-order inference: unification, FOL resolution, Skolemization, soundness/completeness | Apply, Analyze |
| 5 | Rule-based systems: production systems, forward/backward chaining, a Python rule engine | Apply, Create |
| 6 | Semantic networks and frames: inheritance, non-monotonic inheritance, defaults | Apply, Analyze |
| 7 | Description logics and ontologies: DL syntax, DL vs. FOL, OWL/RDF, Semantic Web | Understand, Analyze |
| 8 | Constraint satisfaction in depth: AC-3, backtracking with ordering heuristics; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Non-monotonic reasoning: CWA, default logic, circumscription | Remember–Apply |
| 10 | Planning in depth: STRIPS in depth, partial-order planning, planning graphs (GraphPlan) | Apply, Analyze |
| 11 | Temporal and spatial reasoning: Allen's interval algebra, basic spatial relations | Apply, Analyze |
| 12 | Probabilistic reasoning and Bayesian networks in depth: variable elimination | Apply, Analyze |
| 13 | Reasoning with uncertainty beyond Bayes: Markov logic networks, fuzzy logic | Understand, Apply |
| 14 | Knowledge-based agents in practice: an integrated forward/backward-chaining reasoner | Apply, Create |
| 15 | Current trends: knowledge graphs, neuro-symbolic AI, evaluating KR systems | Understand, Evaluate |
| 16 | Capstone project presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded notebooks/scripts, submitted weekly (Labs 1–15) |
| Assignments (4) | 20% | Problem sets tied to Weeks 4, 7, 10, 14 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Mini-Project | 20% | Proposal (Wk 11) + implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency.

## 8. Tools & Software

- Python 3.10+ (standard library only for every graded component)
- Jupyter Notebook / Google Colab, or a plain text editor and terminal
- No NumPy/pandas/scikit-learn dependency is required anywhere in this course
- Git/GitHub for lab and project submission
- Not required, but discussed as real-world context: Prolog (a logic-programming language built
  directly on unification and resolution) and description-logic/ontology tooling such as the
  Protégé ontology editor or the Pellet DL reasoner (Week 7). Students are never required to
  install or use these; all graded KR algorithms are implemented in Python.

## 9. Reference Textbooks

- Russell, S. & Norvig, P. — *Artificial Intelligence: A Modern Approach* (the logic, planning,
  and constraint-satisfaction chapters are core reading; this course goes beyond their
  survey-level treatment in depth of algorithm and breadth of formalism).
- Brachman, R. & Levesque, H. — *Knowledge Representation and Reasoning* (the course's primary
  KR-specific reference; covers semantic networks, frames, description logics, non-monotonic
  reasoning, and the formal foundations of representation in the depth this course targets).
- van Harmelen, F., Lifschitz, V., & Porter, B. (eds.) — *Handbook of Knowledge Representation*
  (reference chapters on description logics, non-monotonic reasoning, temporal reasoning, and
  constraint satisfaction, drawn on for specific weeks as noted in `course-contents.md`).
- Official Python documentation (language reference and standard library, for lab support only).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done in
pairs with clearly attributed contributions. Code plagiarism (including uncredited AI-generated
code submitted as original work) is handled per institutional academic integrity policy.
