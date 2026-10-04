# Assignment 1 — Expressive Logics: HOL, Many-Valued/Paraconsistent Logics, SROIQ (Weeks 1–4)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 4 |
**Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Simply-typed lambda calculus, 20 pts)** Implement `type_check`, `substitute`, and
   `beta_reduce` exactly as in the Week 2 lecture content. Given `likes : e → e → t`,
   `mary, john : e`, type-check and β-reduce
   `(λx:e. λy:e. likes y x) mary john`, printing the type at every step. In 2–3 sentences, state
   one property this term's construction illustrates that plain FOL cannot express.
2. **(Kleene vs. Łukasiewicz, 20 pts)** Implement the K3 and Ł3 evaluators from Week 3. For the
   formula set `{U→U, U∧(U→F), (U→T)∨F}`, compute and report each formula's value under both
   logics, and in 3–5 sentences explain the single structural reason K3 and Ł3 diverge on `U→U`
   specifically but agree on the other two.
3. **(Paraconsistency, 20 pts)** Implement the Belnap–Dunn FDE evaluator from Week 3. Build a toy
   KB with one contradictory atom (value B) and two unrelated atoms with ordinary evidence.
   Query an unrelated atom and confirm it is unaffected by the contradiction; in 2–3 sentences,
   state what a classical evaluator would instead conclude from the same KB via explosion.
4. **(SROIQ tableau extension, 25 pts)** Implement the fragment checker (`Node`, `role_closure`,
   `propagate_universals`, `check_at_least`, `has_clash`) from Week 4. Given a role hierarchy
   `teaches ⊑ worksWith ⊑ colleague` and a universal restriction `∀colleague.Reviewed` on a node
   with `≥2 teaches.Qualified`, confirm both fresh `teaches`-successors are correctly labeled
   `Reviewed` via two-level role-hierarchy propagation.
5. **(Complexity synthesis, 15 pts)** In 200–300 words, state the complexity results for ALC
   (PSPACE-complete), SHIQ (EXPTIME-complete), and SROIQ (N2ExpTime-complete) precisely, and
   explain specifically which SROIQ constructs (not a vague "more expressive") are responsible
   for each complexity jump.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
