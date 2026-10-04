# Assignment 1 — Modal Logic, Temporal Logic, Description Logics (Weeks 1–4)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 4 |
**Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Kripke semantics, 20 pts)** For a provided 5-world Kripke model (worlds, an accessibility
   relation, and a valuation, given in `assignment01_model.json`), evaluate 4 given modal
   formulas at 2 named worlds using your Week 2 `satisfies` implementation. For each of the 4
   formulas, state by hand which axiom (if any, among T/4/5) its truth value depends on, given
   the model's accessibility-relation properties.
2. **(Correspondence theory, 15 pts)** Prove, by direct semantic argument (not by citing the
   result), that transitivity of R is sufficient for □φ→□□φ to hold at every world. Then give a
   concrete 3-world counterexample model where R is reflexive and symmetric but NOT transitive,
   and show □φ→□□φ fails at some world in it.
3. **(LTL evaluation, 20 pts)** For a provided 8-step execution trace (`assignment01_trace.json`),
   implement and evaluate 3 given LTL formulas (one using G, one using F, one using U) at index
   0 using your Week 3 `ltl_eval`. For each, state in one sentence what real-world planning or
   verification property it could represent.
4. **(ALC tableau, 25 pts)** Given 3 ALC concepts (`assignment01_concepts.json`), determine
   satisfiability using your Week 4 `tableau_satisfiable` implementation. For the one concept
   requiring disjunction branching, show the full hand-worked tableau derivation (which branch
   closes, which stays open) in a markdown cell, matching your code's result.
5. **(DL complexity and OWL profiles, 20 pts)** In 4–6 sentences, state the PSPACE-completeness
   result for ALC satisfiability precisely (what is shown to be in PSPACE, and in what sense it
   is complete for that class — a result-level statement, not a re-derivation of the proof). Then,
   for 3 given application scenarios (a large biomedical terminology, a relational-database-backed
   query system, and a rule-engine-backed large-scale knowledge base), recommend the matching OWL
   2 profile (EL/QL/RL) for each and justify your choice in 1–2 sentences per scenario.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
