# Week 1 — Lecture Content: Graduate KR&R Overview

## 1. What This Course Assumes, and Does Not Re-Teach
This course assumes a completed undergraduate KR&R course (or equivalent). The following are
**assumed knowledge, stated here only as a checklist, not re-derived**:

| Area | Assumed content |
|---|---|
| Logic | Propositional/FOL syntax and semantics, CNF/DNF, resolution refutation, unification, Skolemization |
| Structured representations | Rule-based production systems (forward/backward chaining); semantic networks/frames with non-monotonic inheritance and defaults |
| Description logics | Basic DL syntax (concepts, roles, ⊓, ¬, ∃R.C, ∀R.C), DL-vs-FOL relationship, a toy subsumption check, OWL/RDF basics |
| Constraints | CSP formulation, AC-3 arc consistency, backtracking with ordering heuristics |
| Non-monotonic reasoning | The closed-world assumption, default logic (Reiter), circumscription (conceptual) |
| Planning | STRIPS action schemas, partial-order planning, planning graphs (conceptual) |
| Temporal/spatial | Allen's interval algebra (13 relations), basic spatial topology |
| Probabilistic reasoning | Bayesian networks, exact inference by variable elimination |
| Uncertainty beyond Bayes | A brief MLN and fuzzy-logic survey |
| SAT | DPLL with unit propagation (owned by *Artificial Intelligence*, Graduate, Week 6) |

If any row above is unfamiliar, Lab 1's diagnostic exercises will surface the gap immediately —
raise it with the instructor in Week 1, since every later week builds on this list without
re-explaining it.

## 2. The Advanced-KR Research Map This Course Covers
| Week(s) | Area |
|---|---|
| 2–3 | Modal logic; temporal logic (LTL/CTL) |
| 4–5 | DL tableau algorithm and complexity; sequent calculus, FOL tableau, resolution refinements |
| 6 | Answer Set Programming (stable models) |
| 7–8 | AGM belief revision/update; Dung argumentation frameworks |
| 9 | Markov Logic Networks in depth |
| 10–11 | Knowledge graphs, TransE embeddings, neuro-symbolic KG reasoning |
| 12 | Multi-agent epistemic logic |
| 13–14 | Ontology engineering practice; explainability |
| 15–16 | Research methods; capstone |

## 3. How This Course's Scope Is Disjoint From Its Neighbors
- **Undergraduate KR&R.** Everything in §1's table is assumed; this course never re-derives
  resolution, AC-3, STRIPS, Allen's algebra, or variable elimination.
- ***Artificial Intelligence*, Graduate.** That course owns SAT/DPLL and SMT in depth (its
  Week 6), and owns the game-theoretic, payoff-based treatment of multi-agent systems. This
  course's Week 5 *extends* resolution with refinement strategies rather than re-deriving DPLL,
  and its Week 12 multi-agent-epistemic-logic treatment is explicitly about what agents *know*,
  not about strategic equilibria.
- ***Machine Learning*, Graduate.** That course's Week 11 covers Conditional Random Fields as a
  *discriminative* structured-prediction model. This course's Week 9 MLNs are a first-order
  *probabilistic-logic* formalism — related by both combining logic-like structure with weights,
  but built, used, and reasoned about differently; the two weeks do not overlap in technique.

## 4. Why Graduate Rigor Here Means Something Specific
Three concrete differences from the undergraduate course, visible from Week 2 onward:
1. **Precise formal semantics.** Every new formalism (Kripke models, LTL, the DL tableau, AGM,
   Dung's AF, the MLN log-linear model, TransE, epistemic common/distributed knowledge) is given
   its exact mathematical definition before any code is written.
2. **Real complexity and metatheoretic results are used.** "ALC satisfiability is
   PSPACE-complete" (Week 4) and the AGM postulates (Week 7) are treated as standing results this
   course uses to justify design choices, not as folklore.
3. **Research methods are a graded skill.** Reading and critiquing a real KR paper (from Week 8's
   Dung-paper close reading onward, formalized in Week 15) and producing a literature-review-
   grounded capstone are explicit, graded learning outcomes.

## 5. In-Class Exercise
For each prompt, (a) name which week(s) of this course's map it belongs to, and (b) name one
sibling course it is explicitly NOT about:
1. "Can an agent know a fact that no individual member of its group knows, once they pool
   information?" (Week 12; not *Artificial Intelligence* Graduate's game theory.)
2. "How do we decide, with a sound and complete procedure, whether a description-logic concept
   is satisfiable?" (Week 4; not the undergraduate course's toy semantic subsumption check.)
3. "How should a discriminative model label a sequence of tokens?" (Not this course at all —
   *Machine Learning*, Graduate, Week 11, CRFs.)
