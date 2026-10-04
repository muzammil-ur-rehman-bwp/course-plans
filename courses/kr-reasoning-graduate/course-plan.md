# Course Plan: Knowledge Representation and Reasoning (Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Knowledge Representation and Reasoning |
| Level | Graduate (MS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab/seminar session of 3 hrs/week) |
| Prerequisites | *Knowledge Representation and Reasoning* (undergraduate) or equivalent — propositional/first-order logic and resolution, unification, rule-based systems, semantic networks/frames, description-logic/ontology basics, CSP/AC-3, non-monotonic reasoning basics (default logic/circumscription), STRIPS/HTN planning, Allen's interval algebra, Bayesian networks via variable elimination, and a brief MLN/fuzzy-logic survey — all assumed, not re-taught. Familiarity with SAT/DPLL (as covered in *Artificial Intelligence*, Graduate, Week 6) is also assumed. |
| Programming Language | Python 3.x (used to implement a working, from-scratch core algorithm for nearly every formalism covered) |
| Core Libraries | Python standard library (`itertools`, `collections`, `dataclasses`, `functools`); NumPy for the TransE knowledge-graph-embedding lab only |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab/Seminar (concept lecture followed by a hands-on implementation or research-skills session) |

## 2. Course Description

This is a rigorous, research-oriented graduate treatment of advanced Knowledge Representation
and Reasoning. It assumes students have already completed an undergraduate KR&R course (or
equivalent): propositional and first-order logic with resolution refutation, unification,
rule-based production systems, semantic networks and frames with non-monotonic inheritance,
description-logic and ontology basics, constraint satisfaction (AC-3, backtracking),
non-monotonic reasoning basics (the closed-world assumption, default logic, circumscription),
STRIPS/HTN-style planning, Allen's interval algebra, and exact Bayesian-network inference by
variable elimination, together with a brief survey of Markov logic networks and fuzzy logic. None
of that material is re-taught here; it is the floor this course builds from.

Instead, this course goes where the undergraduate course and its graduate siblings do not. It
covers **modal and temporal logic** (possible-worlds semantics, the correspondence between
accessibility-relation properties and modal systems K/S4/S5, Linear Temporal Logic and a look at
Computation Tree Logic); **automated theorem proving at research depth** (the tableau algorithm
for description-logic concept satisfiability and its complexity, the sequent calculus, first-order
tableau, and resolution refinement strategies — set-of-support and ordering restrictions — that go
beyond the undergraduate course's basic resolution and beyond the DPLL/SAT depth owned by
*Artificial Intelligence*, Graduate, Week 6); **non-monotonic reasoning via Answer Set
Programming** (stable-model semantics, contrasted with the undergraduate course's default logic);
**belief revision and update** (the AGM postulates); **abstract argumentation** (Dung's frameworks
and their acceptability semantics); **Markov Logic Networks in genuine depth** (the log-linear
model over possible worlds, grounding, and inference — well past the undergraduate course's brief
survey); **knowledge graphs and embeddings** (RDF triples, the TransE model, link prediction, and
a grounded look at neuro-symbolic reasoning); **multi-agent epistemic logic** (common and
distributed knowledge, the muddy-children puzzle); **ontology engineering practice**; and
**explainability in symbolic reasoning**, contrasted briefly and honestly with black-box ML
explainability.

**This course is deliberately scoped to avoid duplicating its siblings.** It is not a repeat of
the undergraduate course's foundations (propositional/FOL resolution, CSP, STRIPS planning,
Allen's algebra, and variable elimination are assumed, not re-derived). It does not re-derive
DPLL/SAT or give a general SMT treatment — that depth belongs to *Artificial Intelligence*,
Graduate, Week 6; this course's automated-theorem-proving week extends resolution with refinement
strategies and introduces tableau/sequent methods instead. Its multi-agent-systems week is about
what agents *know and believe*, not about the game-theoretic, payoff-based multi-agent treatment
in *Artificial Intelligence*, Graduate — the two are explicitly contrasted, not merged. Its Markov
Logic Networks week is not a repeat of *Machine Learning*, Graduate's Conditional-Random-Field
angle on structured prediction (a discriminative sequence-labeling model); MLNs here are treated
as a first-order probabilistic-logic formalism in their own right.

## 3. Goals

- Master possible-worlds (Kripke) semantics for modal logic and the correspondence between
  accessibility-relation properties and modal systems (K, S4, S5), including epistemic readings.
- Represent and reason about time with Linear Temporal Logic and (at a conceptual level)
  Computation Tree Logic, and connect both to planning and verification.
- Work the tableau algorithm for description-logic concept satisfiability by hand and in code, and
  state the correct complexity results for DL reasoning and the OWL 2 tractable profiles.
- Extend first-order automated theorem proving beyond basic resolution: the sequent calculus,
  first-order tableau, and resolution refinement strategies (set-of-support, ordering).
- Formalize and compute stable models for Answer Set Programs, and use ASP to solve a small
  combinatorial problem, contrasting it with the undergraduate course's default logic.
- Apply the AGM postulates to characterize rational belief revision, and distinguish revision from
  update.
- Compute the grounded and preferred extensions of a Dung abstract argumentation framework.
- Formulate Markov Logic Networks precisely as weighted first-order formulas inducing a log-linear
  distribution over possible worlds, and reason (conceptually and on toy examples) about inference.
- Represent knowledge as RDF triples, train and query a toy TransE knowledge-graph embedding for
  link prediction, and describe grounded neuro-symbolic approaches to reasoning over graphs.
- Formalize common and distributed knowledge in multi-agent epistemic logic and work the
  muddy-children puzzle precisely.
- Describe ontology-engineering methodology and ontology alignment, and contrast symbolic
  explanation (proof trees) with black-box ML explainability.
- Read and critique a real KR research paper, and design, execute, and present an original
  research-style capstone project on an advanced KR subtopic from this course.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the undergraduate KR&R foundations at graduate pace and map the landscape of advanced KR research this course covers, distinguishing it from sibling graduate courses. | Remember, Understand |
| CLO2 | Evaluate modal-logic formulas against a Kripke model and relate accessibility-relation properties to the modal systems K/S4/S5 and their epistemic reading. | Apply, Analyze |
| CLO3 | Evaluate Linear Temporal Logic formulas against an execution trace and describe branching-time (CTL) reasoning, connecting both to planning and verification. | Apply, Analyze |
| CLO4 | Run the tableau algorithm to decide ALC concept satisfiability by hand and in code, and state accurate complexity results for DL reasoning and the OWL 2 profiles. | Apply, Analyze |
| CLO5 | Construct sequent-calculus and first-order-tableau derivations, and apply resolution refinement strategies (set-of-support, ordering) to reduce search. | Apply, Analyze |
| CLO6 | Compute the stable models of a small Answer Set Program and encode a combinatorial problem as one, contrasting ASP's negation-as-failure with classical non-monotonic formalisms. | Apply, Analyze |
| CLO7 | Apply the AGM postulates to evaluate whether a belief-change operator is a rational revision, and distinguish revision from update. | Apply, Analyze, Evaluate |
| CLO8 | Compute the grounded and preferred extensions of a Dung argumentation framework and use it to reason about conflicting information. | Apply, Analyze |
| CLO9 | Formulate a Markov Logic Network's log-linear distribution precisely, ground a small MLN, and reason about relative world probabilities. | Apply, Analyze, Create |
| CLO10 | Represent knowledge as RDF triples, train a toy TransE embedding, and use it for link prediction; describe grounded neuro-symbolic reasoning over knowledge graphs. | Apply, Analyze, Create |
| CLO11 | Formalize common and distributed knowledge in multi-agent epistemic logic and correctly work the muddy-children puzzle. | Apply, Analyze |
| CLO12 | Critique a real KR research paper's claims and methodology, and design, execute, and present an original research-style capstone on an advanced KR subtopic. | Analyze, Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Graduate Foundations & Modal/Temporal Logic | 1–3 | Remember, Understand, Apply | Research landscape; Kripke semantics and modal systems; LTL/CTL |
| Automated Reasoning at Research Depth | 4–6 | Apply, Analyze | DL tableau and complexity; sequent calculus/FOL tableau/resolution refinements; ASP stable models |
| Belief Change, Argumentation & Statistical Relational Reasoning | 7–9 | Apply, Analyze, Evaluate | AGM revision/update; Dung argumentation; midterm; MLNs in depth |
| Knowledge Graphs & Multi-Agent Epistemics | 10–12 | Apply, Analyze, Create | RDF/TransE; neuro-symbolic KG reasoning; common/distributed knowledge |
| Practice, Explainability & Research Synthesis | 13–16 | Analyze, Evaluate, Create | Ontology engineering; explainability; research methods; capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Graduate KR&R overview: rapid review of assumed foundations, landscape of advanced KR research | Remember, Understand |
| 2 | Modal logic: Kripke semantics, accessibility relations, K/S4/S5, epistemic reading | Understand, Apply |
| 3 | Temporal logic: LTL syntax/semantics, a look at CTL, applications to planning/verification | Apply, Analyze |
| 4 | Description logics in depth: the ALC tableau algorithm, DL complexity, OWL 2 profiles | Apply, Analyze |
| 5 | Automated theorem proving in depth: sequent calculus, FOL tableau, resolution refinements | Apply, Analyze |
| 6 | Non-monotonic reasoning via ASP: stable-model semantics, ASP syntax, graph coloring | Apply, Analyze |
| 7 | Belief revision and update: the AGM postulates, revision vs. update, a worked example | Apply, Analyze |
| 8 | Argumentation frameworks: Dung's AF, grounded/preferred extensions; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Markov Logic Networks in depth: log-linear model, grounding, inference | Remember–Apply |
| 10 | Knowledge graphs: RDF triples, TransE embeddings, link prediction | Apply, Analyze |
| 11 | Reasoning over knowledge graphs: rule mining, neuro-symbolic reasoning | Apply, Analyze |
| 12 | Multi-agent epistemic reasoning: common/distributed knowledge, the muddy-children puzzle | Apply, Analyze |
| 13 | Ontology engineering in practice: methodology, alignment, reasoner tooling | Understand, Apply |
| 14 | Explainability and reasoning: proof trees vs. black-box ML explainability | Understand, Analyze |
| 15 | Research methods and project work session | Analyze, Evaluate |
| 16 | Capstone research presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 15% | Graded notebooks/scripts, submitted weekly (Labs 1–15) |
| Assignments (3 problem sets) | 15% | Tied to Weeks 4, 8, 12 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-methods assignment; written critique + presentation during Week 15 seminar |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Research Capstone | 25% | Literature review + reproduced/extended experiment + paper + presentation (topic selection Wk 7–8, proposal due Wk 8, work session Wk 15, presentation Wk 16) |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–15 |

This weighting mirrors the sibling graduate courses in this program: exams are de-emphasized
relative to an undergraduate course (Midterm + Final total 25%) in favor of sustained,
research-style work — a dedicated Paper Critique & Presentation component (10%) and a heavy
Research Capstone (25%) that requires a real literature review and experiment, not just an
implementation.

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (proposal, draft, final submission) have fixed
deadlines because of the downstream presentation schedule; late capstone milestones are handled
case-by-case with the instructor.

## 8. Tools & Software

- Python 3.10+ (standard library for nearly every graded component)
- Jupyter Notebook / Google Colab, or a plain text editor and terminal
- NumPy, for the Week 10 toy TransE embedding lab only
- An Answer Set Programming solver such as `clingo` is discussed conceptually in Week 6 as what
  production ASP solving looks like beyond a teaching-scale brute-force stable-model checker; it
  is **not** required to be installed, and all graded ASP code in this course is a from-scratch
  Python stable-model checker over small programs
- A description-logic reasoner such as Pellet or HermiT is discussed conceptually in Weeks 4 and
  13 as real-world DL tooling; students are never required to install or use one, since all graded
  DL-tableau code in this course is a from-scratch Python implementation over a small ALC fragment
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks

- Brachman, R. & Levesque, H. — *Knowledge Representation and Reasoning* (the course's anchor KR
  text; this course extends its description-logic, non-monotonic-reasoning, and argumentation-
  adjacent material to graduate depth and into territory — modal/temporal logic, ASP, AGM belief
  revision, multi-agent epistemic logic — the book introduces more briefly or not at all).
- Baader, F., Calvanese, D., McGuinness, D., Nardi, D. & Patel-Schneider, P. (eds.) — *The
  Description Logic Handbook* (the primary reference for Week 4's tableau algorithm, DL complexity
  results, and Week 13's ontology-engineering material).
- Gelfond, M. & Kahl, Y. — *Knowledge Representation, Reasoning, and the Design of Intelligent
  Agents: The Answer-Set Programming Approach* (the primary reference for Week 6's stable-model
  semantics and ASP).
- Fagin, R., Halpern, J., Moses, Y. & Vardi, M. — *Reasoning About Knowledge* (the canonical
  reference for Week 12's multi-agent epistemic logic, common/distributed knowledge, and the
  muddy-children puzzle).
- Current papers from venues such as KR, IJCAI/AAAI (KR tracks), and the journal *Artificial
  Intelligence* are assigned as readings from Week 13 onward (research-methods and ontology/
  explainability weeks).
- Official Python documentation (language reference and standard library, for lab support only).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research capstone may be done in
pairs with clearly attributed contributions. Any use of another author's ideas, text, code, or
results — including figures or results from a paper being critiqued or reproduced — must be
properly cited; uncredited reuse of a paper's text or another student's code (including
uncredited AI-generated code or text submitted as original work) is handled per institutional
academic integrity policy. Reproducing a published result at small scale for the capstone is
expected and encouraged; presenting someone else's reported results as your own experimental
findings is not.
