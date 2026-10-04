# Assignment 3 — Constraints, Non-Monotonic Reasoning & Planning (Weeks 8–10)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 10 | **Due:** Start of Week 12

## Instructions
Submit `assignment03.ipynb` (or notebook + short written answers where indicated) with working
code and written answers for all questions.

## Questions
1. **(AC-3, 20 pts)** For a 5-variable CSP (graph and constraints provided), trace AC-3 by hand
   for the first 4 arc revisions, then run your Lab 8 `ac3` implementation and confirm your
   domains match.
2. **(Backtracking Heuristics, 15 pts)** On the same CSP, apply MRV to choose the first 2
   variables by hand, and LCV to order the first chosen variable's values; verify with your Lab 8
   `backtracking_search`.
3. **(Non-Monotonic Reasoning, 25 pts)** Design a default-logic scenario (not bird/penguin) with
   2 defaults, where adding one new fact blocks exactly one default's justification. Using your
   Lab 9 `apply_defaults`, show the derived set both before and after the new fact, and explain
   in 2–3 sentences why this is impossible for a Week 5 strict-rule engine.
4. **(Planning in Depth, 25 pts)** Define a toy domain with 2 variabilized STRIPS schemas; ground
   them over a 3-object domain by hand (list all resulting actions), then use your Lab 10
   partial-order planner to build a 3-step plan, showing at least one causal link and (if
   applicable) one resolved threat.
5. **(Synthesis, 15 pts)** In 1–2 paragraphs, compare arc consistency (Week 8) and path
   consistency (Week 11, previewed) as two instances of the same general idea — propagating local
   constraints to prune what is not yet ruled impossible — and discuss one way each can fail to
   detect a global inconsistency despite being locally consistent.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
