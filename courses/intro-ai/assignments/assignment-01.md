# Assignment 1 — Agents & Search (Weeks 1–4)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit `assignment01.ipynb` (or a combination of notebook + short written answers where
indicated) with working code and written answers for all questions.

## Questions
1. **(PEAS & Agents, 15 pts)** Choose a task environment not covered in lecture (e.g., a
   drone delivery system, an online tutoring system). Write its PEAS specification, classify it
   against all six environment properties from Week 2, and justify each classification in 1–2
   sentences.
2. **(Search Formulation, 15 pts)** Formulate the 8-puzzle as a search `Problem` (states,
   actions, transition model, goal test, path cost). Implement it as a Python class.
3. **(Uninformed Search, 20 pts)** Solve a given 8-puzzle instance with BFS, DFS, and
   uniform-cost search; report the solution path length and nodes expanded for each. Explain any
   difference in path length between BFS and DFS.
4. **(Heuristics & A\*, 25 pts)** Propose two different heuristics for the 8-puzzle (e.g.,
   misplaced-tiles count, total Manhattan distance). For each, argue whether it is admissible.
   Implement A* using each heuristic; report path length and nodes expanded. Which heuristic
   performs better, and why does that follow from admissibility/informedness?
5. **(Local Search, 25 pts)** For a toy optimization problem of your choosing (e.g., minimizing a
   simple multi-peaked function), implement hill climbing and simulated annealing. Run each from
   3 different starting points and report, in a short table, how often each finds the global
   optimum. Discuss the result in 3–5 sentences.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
