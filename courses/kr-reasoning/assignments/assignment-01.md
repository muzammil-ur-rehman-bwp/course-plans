# Assignment 1 — Logic Foundations (Weeks 1–4)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit `assignment01.ipynb` (or a combination of notebook + short written answers where
indicated) with working code and written answers for all questions.

## Questions
1. **(KR Desiderata, 10 pts)** For a domain of your choosing (not used in lecture or lab), sketch
   one piece of knowledge represented three ways (flat English, a triple-store fact set, a FOL
   sentence) and classify each against expressiveness, inferential efficiency, and naturalness.
2. **(Normal Forms & Resolution, 25 pts)** Given 5 propositional clauses and a query (provided by
   the instructor), convert any non-clausal sentences to CNF by hand, then perform resolution
   refutation by hand to prove or disprove the query. Verify your result using your Lab 2
   `resolution_refutation` implementation.
3. **(SAT, 15 pts)** For the same 5-clause set plus negated query, run your Lab 2
   `is_satisfiable` checker and confirm its result is consistent with your resolution answer
   (unsatisfiable exactly when the query is proved). Time the checker on a randomly generated
   20-symbol clause set and report the runtime.
4. **(First-Order Translation, 25 pts)** Translate 4 given English sentences into FOL, including
   at least one requiring nested quantifiers and one requiring the implication-not-conjunction
   pattern for "all P are Q." For one sentence, build a 3-object model by hand and evaluate it.
5. **(Unification & FOL Resolution, 25 pts)** Given two FOL clauses requiring a non-trivial
   unification (provided by the instructor), compute the MGU by hand, then resolve the clauses by
   hand, applying the substitution to the full resolvent. Verify using your Lab 4 `unify` and
   `fol_resolve` implementations.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
