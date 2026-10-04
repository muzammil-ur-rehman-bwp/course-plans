# Course Plan: Advanced Knowledge Representation and Reasoning (Post Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Advanced Knowledge Representation and Reasoning |
| Level | Post Graduate (PhD-track: PhD coursework, or an MS student heading toward a thesis) |
| Credit Hours | 3 (2 hrs lecture + 1 research seminar/lab session of 3 hrs/week) |
| Prerequisites | *Knowledge Representation and Reasoning* (Graduate), or equivalent: modal and temporal logic (Kripke semantics, K/S4/S5, LTL), the description-logic tableau method for **ALC** and its complexity, Answer Set Programming/stable-model semantics, the AGM postulates for belief revision, Dung's abstract argumentation framework (grounded/preferred extensions), Markov Logic Networks in depth (log-linear models over possible worlds, grounding), knowledge graphs and embeddings (RDF, TransE), multi-agent epistemic logic (common/distributed knowledge), ontology engineering practice, and explainability in symbolic systems (proof trees vs. black-box ML explanation). Every one of these is assumed as firm foundation and is **not** re-taught here. |
| Programming Language | Python 3.x (every formalism in this course is probed with a from-scratch implementation; NumPy/PyTorch are used where differentiable computation is the point) |
| Core Libraries | Python standard library (`itertools`, `dataclasses`, `functools`, `collections`); NumPy for the probabilistic/weighted-inference labs; PyTorch (or NumPy with manual gradients) for the differentiable-logic lab |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + research seminar/lab (concept lecture followed by an implementation, critical-writing, or proposal-development session, depending on the week) |

## 2. Course Description

This is a PhD-track, research-frontier treatment of Knowledge Representation and Reasoning that
assumes the graduate KR&R course in full — modal/temporal logic, the ALC tableau method, Answer
Set Programming, AGM belief revision, Dung's abstract argumentation, Markov Logic Networks in
depth, knowledge-graph embeddings, multi-agent epistemic logic, ontology engineering, and
symbolic explainability — as settled background, and spends every week building past it into
territory the graduate course does not cover.

The course has four pillars. **(1) Expressive logics beyond the graduate course's ALC**:
higher-order logic and the simply-typed lambda calculus as a foundation for more expressive
representation; many-valued and paraconsistent logics for reasoning under genuine inconsistency;
and **SROIQ**, the expressive description logic underlying OWL 2 DL, which extends ALC with role
hierarchies, complex role inclusions, number restrictions, and nominals. **(2) Structured and
quantitative non-classical reasoning**: the **ASPIC+** framework for *structured* argumentation,
building concrete arguments from inference rules and premises rather than treating arguments as
the graduate course's unstructured Dung nodes; and probabilistic logic programming via the
**distribution semantics** (as in ProbLog), which attaches probabilities directly to logic-program
facts and builds on the graduate course's MLN depth from a different, program-oriented angle.
**(3) Neuro-symbolic integration and formal verification as research frontiers**: differentiable
relaxations of logical operators that make symbolic rules learnable by gradient descent; a
conceptual survey of embedding-based neural theorem proving; and model checking, which extends
the graduate course's LTL introduction into a rigorous decision procedure for verifying that a
knowledge base or agent specification satisfies a temporal property. **(4) Multi-agent,
explanatory, and methodological research topics**: belief *merging* across multiple agents with
conflicting beliefs (extending the graduate course's single-agent AGM revision); explanation and
justification generation for expressive-DL entailments as an actively researched KR problem in
its own right; ontology evolution and versioning as a grounded engineering/research challenge; and
a survey of currently open problems in KR research.

The capstone reflects what postgraduate work is for. Where the graduate course's capstone is a
literature review plus a small reproduced or extended experiment, this course's capstone is a
**research proposal** — a PhD-qualifying-exam-style deliverable: a precise, falsifiable problem
statement, a related-work survey of five or more papers, a proposed novel approach or extension,
and either preliminary results or a rigorous feasibility argument. It is evaluated as a
thesis-proposal committee would evaluate it, not as a course project.

**This course does not repeat the graduate KR&R course.** The ALC tableau method, basic ASP,
AGM revision, Dung's abstract argumentation, MLNs' log-linear formulation, RDF/TransE, and
single-agent epistemic logic are assumed fluent and referenced only as background (e.g., "recall
the ALC tableau rules from the graduate course" when extending them to SROIQ). It does not
duplicate the sibling postgraduate courses either: deep architectures and training (*Advanced
Artificial Neural Network*, *Advanced Deep Learning*), statistical/algorithmic learning theory
(*Advanced Machine Learning*), or the regret-theoretic, multi-agent-game-theoretic, and
alignment/interpretability material of *Advanced Artificial Intelligence* — where one of those
topics is relevant to an argument here (e.g., gradient descent in the neuro-symbolic weeks), it
receives at most a one-sentence pointer, never a lecture.

## 3. Goals

- Introduce higher-order logic and the simply-typed lambda calculus as a response to first-order
  logic's inability to quantify over predicates/relations themselves, and connect this to typed
  representations in modern KR systems.
- Formalize and apply three-valued logics (Kleene, Łukasiewicz) and paraconsistent logics, and use
  them to reason over genuinely inconsistent information sources without trivializing via
  explosion.
- Extend the graduate course's ALC tableau method to **SROIQ** (role hierarchies, complex role
  inclusion axioms, number restrictions, nominals), and state the resulting complexity/decidability
  picture accurately.
- Construct structured arguments in the **ASPIC+** framework from rules and premises, and
  distinguish rebutting attacks (on a conclusion) from undercutting attacks (on a rule's
  applicability).
- Formalize the distribution semantics for probabilistic logic programs (à la ProbLog), building
  on the graduate course's MLN depth, and reason about inference in small probabilistic programs.
- Implement differentiable/fuzzy relaxations of logical connectives and explain why making logic
  differentiable enables gradient-based learning of symbolic rules from data; survey
  embedding-based neural theorem proving and KG-embedding-plus-constraint hybrids.
- State the model-checking problem precisely and check whether a small knowledge base or agent
  specification satisfies a temporal/modal property, building rigorously on the graduate course's
  LTL introduction.
- Extend AGM revision to multi-agent belief **merging**, and state the postulates a rational
  merging operator should satisfy.
- Explain why justification/explanation generation for expressive-DL entailments is an open KR
  research problem, and construct proof-based explanations for small SROIQ-style entailments.
- Describe the real engineering and research challenges of ontology evolution and versioning.
- Survey 2–3 currently active open problems in KR research relevant to this course, explicitly
  flagged as a fast-moving area.
- Scope, write, and defend an original PhD-qualifying-style research proposal: a problem
  statement, a 5+ paper related-work survey, a proposed novel approach, and a feasibility argument
  or preliminary result.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the graduate KR&R formalism this course assumes, and map the postgraduate research-frontier landscape (expressive logics, structured/quantitative non-classical reasoning, neuro-symbolic integration, formal verification, multi-agent/explanatory topics) this course covers. | Remember, Understand |
| CLO2 | Explain the expressive limits of first-order logic that motivate higher-order logic and the simply-typed lambda calculus, and connect typed representation to modern KR systems. | Understand, Analyze |
| CLO3 | Apply Kleene's and Łukasiewicz's three-valued truth tables and a paraconsistent logic to a genuinely inconsistent knowledge source, and contrast their behavior under negation and implication. | Apply, Analyze |
| CLO4 | Extend the ALC tableau method to decide satisfiability under SROIQ's additional constructs (role hierarchies, complex role inclusions, number restrictions, nominals), and state the correct complexity/decidability results. | Apply, Analyze, Evaluate |
| CLO5 | Construct ASPIC+ structured arguments from rules and premises and correctly classify an attack as rebutting or undercutting. | Apply, Analyze |
| CLO6 | Formulate the distribution semantics for a probabilistic logic program, compute the probability of a query by summing over consistent choices of probabilistic facts, and relate this formalism to the graduate course's MLN semantics. | Apply, Analyze |
| CLO7 | Implement differentiable/fuzzy relaxations of conjunction, disjunction, and negation, and explain how they enable gradient-based rule learning; critically evaluate embedding-based neural theorem proving and KG-embedding/constraint hybrids. | Apply, Analyze, Evaluate |
| CLO8 | State the model-checking problem precisely and determine, for a small finite-state system and an LTL property, whether the system satisfies it. | Apply, Analyze |
| CLO9 | Extend AGM belief revision to multi-agent belief merging and evaluate a merging operator against the standard merging postulates. | Apply, Analyze, Evaluate |
| CLO10 | Construct and critique justification/proof-based explanations for expressive-DL entailments, and articulate why ontology-entailment explanation remains an open research problem. | Analyze, Evaluate |
| CLO11 | Describe grounded ontology-evolution/versioning challenges and critically survey 2–3 currently open problems in KR research. | Understand, Analyze, Evaluate |
| CLO12 | Formulate an original KR research question, survey and synthesize 5+ related papers, and construct a rigorous feasibility argument or preliminary result for a proposed novel approach. | Analyze, Evaluate, Create |
| CLO13 | Design, write, and orally defend a PhD-qualifying-exam-style research proposal under questioning, in the manner of a thesis-proposal committee. | Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Postgraduate Orientation | 1 | Remember, Understand | Research-frontier landscape; assumed-foundations review; introducing proposal scoping |
| Expressive Logics: Higher-Order, Many-Valued & Description Logics | 2–4 | Understand, Apply, Analyze | HOL/type theory; Kleene/Łukasiewicz and paraconsistent logics; SROIQ tableau extension |
| Structured Argumentation & Probabilistic Logic Programming | 5–6 | Apply, Analyze | ASPIC+ rebutting/undercutting attacks; ProbLog-style distribution semantics |
| Neuro-Symbolic Integration & Formal Verification | 7–9 | Apply, Analyze, Evaluate | Differentiable/fuzzy logic; neural theorem proving; midterm; model checking |
| Belief Merging, Explanation & Ontology Evolution | 10–12 | Analyze, Evaluate | Multi-agent belief merging; DL justification/explanation; ontology evolution |
| Research Methods, Open Problems & Capstone | 13–16 | Evaluate, Create | Reading/critiquing frontier papers; open-problems survey; proposal drafting and defense |

Note the same deliberate skew as the sibling postgraduate courses: **Evaluate** and **Create**
dominate from the midpoint of the semester onward, and the capstone (CLO12–CLO13, both
Evaluate/Create) is weighted accordingly — postgraduate work is judged chiefly on the ability to
formulate and scope original research, not on recall or routine application.

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Postgraduate overview: the advanced-KR research landscape; rapid review of assumed graduate foundations; how to scope a research proposal (introduced early) | Remember, Understand |
| 2 | Higher-order logic and type theory: expressive limits of FOL; the simply-typed lambda calculus; connections to typed KR representations | Understand, Analyze |
| 3 | Many-valued and paraconsistent logics: Kleene vs. Łukasiewicz three-valued logics; tolerating contradiction without explosion | Apply, Analyze |
| 4 | Advanced description logics: SROIQ (role hierarchies, complex role inclusions, number restrictions, nominals); extending the ALC tableau; complexity/decidability | Apply, Analyze |
| 5 | Structured argumentation: the ASPIC+ framework; rebutting vs. undercutting attacks | Apply, Analyze |
| 6 | Probabilistic logic programming: the distribution semantics (ProbLog-style); building on MLN depth; inference (conceptual) | Apply, Analyze |
| 7 | Neuro-symbolic integration I: differentiable/fuzzy relaxations of logical operators; gradient-based rule learning | Apply, Analyze |
| 8 | Neuro-symbolic integration II: neural theorem proving (conceptual); KG embeddings plus logical constraints; midterm review | Apply, Analyze, Evaluate |
| 9 | **Midterm Exam** + Formal verification: model checking fundamentals, extending the graduate course's LTL introduction | Apply, Analyze |
| 10 | Multi-agent belief merging: extending AGM to multiple agents; merging operators and postulates (conceptual) | Apply, Analyze |
| 11 | Explanation and justification research: proof-tree explanation for expressive-DL entailments as an open KR problem | Analyze, Evaluate |
| 12 | Ontology evolution and versioning: handling change over time; grounded engineering/research challenges | Understand, Analyze |
| 13 | Research methods for advanced KR research: reading/critiquing frontier papers; structured capstone work time | Analyze, Evaluate |
| 14 | Current open problems survey (grounded, explicitly flagged as fast-moving) | Understand, Evaluate |
| 15 | Research proposal work session: drafting, refining, peer feedback | Apply, Create |
| 16 | Capstone research proposal presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 10% | Graded notebooks/exercises, Labs 1–15 |
| Assignments (2 problem sets) | 10% | Tied to Weeks 4 and 8 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-skills assignment; written critique + in-class presentation of a current KR paper |
| Midterm Exam | 10% | Week 9, qualifying-exam style, covers Weeks 1–8 |
| Research Proposal Capstone | 40% | Problem statement, 5+ paper related-work survey, proposed novel approach, feasibility argument/preliminary results, written proposal, and oral defense |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–14 |

**Rationale for the postgraduate weighting.** This mirrors the sibling postgraduate courses
exactly. Exams (Midterm + Final) total only 20%, and weekly Labs are 10% because postgraduate lab
sessions are shorter, more exploratory, and increasingly folded into proposal-development work by
the back half of the semester. The Research Proposal Capstone, at 40%, is by far the largest
single component and is deliberately judged like a thesis-proposal defense: postgraduate work
should be judged mostly on the ability to formulate and scope original research — a sound problem
statement, a genuine survey of the related work, a defensible proposed approach, and an honest
feasibility argument — not on exam recall or on completing a fixed, instructor-specified
assignment. The components total exactly 100% (10 + 10 + 10 + 10 + 10 + 40 + 10 = 100).

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (problem-statement check-in Week 8, draft proposal Week
15, final proposal Week 15/16, defense Week 16) have fixed deadlines because of the downstream
defense schedule; late capstone milestones are handled case-by-case with the instructor.

## 8. Tools & Software

- Python 3.10+ (standard library for most graded components)
- Jupyter Notebook / Google Colab, or a plain text editor and terminal
- NumPy, for the probabilistic-logic-programming and knowledge-graph-adjacent computations
- PyTorch (or NumPy with manually coded gradients, as a fallback), for the Week 7 differentiable-
  logic lab only
- An Answer Set Programming solver such as `clingo` and a probabilistic logic programming system
  such as `problog` are discussed conceptually, as what production tooling looks like beyond this
  course's teaching-scale implementations; neither is **required** to be installed, since all
  graded code in this course is a from-scratch Python implementation over small instances
- A description-logic reasoner such as Pellet, HermiT, or FaCT++ is discussed conceptually in
  Weeks 4 and 11 as real-world SROIQ/OWL 2 DL tooling; students are never required to install or
  use one, since all graded tableau code in this course is a from-scratch Python implementation
  over a small, fixed fragment
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks and Reading

- Baader, F., Calvanese, D., McGuinness, D., Nardi, D. & Patel-Schneider, P. (eds.) — *The
  Description Logic Handbook* (the primary reference for Week 4's extension from ALC to SROIQ,
  its complexity results, and Week 11's explanation/justification material; the course's anchor
  text for description-logic depth, continuing from where the graduate course's Week 4 left off).
- Modgil, S. & Prakken, H. — the ASPIC+ structured-argumentation literature (the primary reference
  for Week 5's rules/premises/rebutting/undercutting framework; also see Prakken & Vreeswijk's
  survey work on logics for structured argumentation).
- De Raedt, L., Kimmig, A., and co-authors — the probabilistic logic programming / ProbLog
  literature (the primary reference for Week 6's distribution semantics and its relation to the
  graduate course's MLN formulation).
- **Current papers from the KR, IJCAI, and AAAI conferences (and the journal *Artificial
  Intelligence*) are the primary reading material from Week 7 onward** (neuro-symbolic
  integration, formal verification for KR, belief merging, explanation research, ontology
  evolution, and the open-problems survey). This is a fast-moving research area; the instructor
  selects and refreshes the specific paper list each offering rather than this syllabus naming a
  fixed set that would quickly date. Students are expected to locate, read, and critically
  evaluate current primary sources as a core postgraduate skill, not merely consume a fixed
  reading list.
- Official Python, NumPy, and PyTorch documentation (for lab support only).

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research proposal capstone is
individual work, reflecting its role as a qualifying-exam-style assessment of each student's own
ability to formulate research; any collaboration on the capstone (e.g., discussing a problem
statement with a peer) must be disclosed in the proposal's acknowledgments. Postgraduate work is
held to an **original-contribution standard**: a related-work survey must accurately represent
what each cited paper actually claims and found, a proposed approach must be the student's own
formulation (extending or combining existing ideas in a way the student can clearly explain and
defend, not a restatement of a single paper's contribution as if it were novel), and any
feasibility argument or preliminary result must be the student's own reasoning or own
experimentation. Uncredited reuse of another author's ideas, text, code, or results — including
uncredited AI-generated text or code submitted as original work, or presenting a paper's reported
results as the student's own preliminary findings — is handled per institutional academic
integrity policy and, for the capstone specifically, is treated with the same seriousness as
plagiarism in a thesis proposal.
