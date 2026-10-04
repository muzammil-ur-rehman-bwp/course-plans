# Assignment 2 — Adversarial Search & Logic (Weeks 5–8)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 8 | **Due:** Start of Week 10

## Instructions
Submit `assignment02.ipynb` (or notebook + short written answers where indicated) with working
code and written answers for all questions.

## Questions
1. **(Minimax & Alpha-Beta, 20 pts)** For a given 3-ply Tic-Tac-Toe subtree (provided as a
   diagram), trace minimax by hand to find the value of the root and the best move. Then trace
   alpha-beta pruning on the same subtree, showing which branches are pruned and why.
2. **(Propositional Logic, 20 pts)** For three given propositional sentences, build full truth
   tables by hand and classify each as valid, satisfiable-but-not-valid, or unsatisfiable. Verify
   your answers using the `truth_table`/`is_valid`/`is_satisfiable` functions from Lab 6.
3. **(Resolution, 20 pts)** Given a small knowledge base (4–5 propositional clauses) and a query,
   convert the KB to CNF if needed, then perform resolution refutation by hand to prove or
   disprove the query. Verify your result using your Lab 7 resolution implementation.
4. **(First-Order Logic, 20 pts)** Translate 4 given English sentences into FOL, including at
   least one requiring nested quantifiers. For one sentence, construct a tiny 2-object model and
   show by hand whether your FOL translation evaluates to true in that model.
5. **(Synthesis, 20 pts)** In 1–2 paragraphs, compare the role of "search" in Weeks 3–5
   (uninformed/informed/adversarial) to the role of "search" implicit in resolution's repeated
   attempts to find resolvable clause pairs (Week 7). What is being searched over in each case?

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
