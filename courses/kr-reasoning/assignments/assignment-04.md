# Assignment 4 — Temporal/Probabilistic Reasoning & KR Practice (Weeks 11–14)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 14 | **Due:** Start of Week 16

## Instructions
Submit `assignment04.ipynb` (or notebook + short written answers where indicated) with working
code and written answers for all questions.

## Questions
1. **(Allen's Interval Algebra, 20 pts)** Given 4 intervals (provided by the instructor), compute
   all `C(4,2) = 6` pairwise Allen relations by hand, then verify each with your Lab 11
   `allen_relation` function.
2. **(Path Consistency, 15 pts)** For a 3-interval network with 2 known and 1 partially-known
   relation (provided), run your Lab 11 `propagate_path_consistency` and report which relations,
   if any, get tightened.
3. **(Variable Elimination, 25 pts)** Given a 4-node Bayesian network (structure and CPTs
   provided), compute `P(query | evidence)` by hand using variable elimination — writing out
   each factor created and which variable is summed out at each step — then verify with your
   Lab 12 `variable_elimination` implementation.
4. **(Fuzzy Logic, 15 pts)** Given a triangular membership function and 3 input values, compute
   membership degrees by hand; combine two of them with fuzzy AND and fuzzy OR; verify with your
   Lab 13 functions.
5. **(Integrated Agent, 25 pts)** Extend the toy knowledge-based agent from Lab 14 with 2 new
   rules and 1 new frame, such that at least one new rule's premise can only be satisfied via the
   new frame. Query the agent and produce a full derivation trace for the result. In 2–3
   sentences, explain which Week 15 evaluation axis (correctness, scalability, explainability)
   this trace most directly supports.

## Submission
Upload `assignment04.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
