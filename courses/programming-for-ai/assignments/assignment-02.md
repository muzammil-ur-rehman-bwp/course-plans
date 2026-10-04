# Assignment 2 — Search Algorithms (Weeks 5–6)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 6 | **Due:** Start of Week 8

## Instructions
Submit `assignment02.ipynb` with working code and written answers for all questions.

## Questions
1. **(Formulation, 15 pts)** Formulate the 8-puzzle as a search `Problem` (states, actions,
   transition model, goal test). Implement it as a Python class.
2. **(BFS/DFS, 20 pts)** Solve a given 8-puzzle instance with both BFS and DFS; report the
   solution path length and nodes expanded for each. Explain any difference in path length.
3. **(Heuristics, 20 pts)** Propose two different heuristics for the 8-puzzle (e.g., misplaced
   tiles count, total Manhattan distance). For each, argue whether it is admissible.
4. **(A\*, 25 pts)** Implement A* using each heuristic from Question 3; solve the same puzzle
   instance; report path length and nodes expanded for each heuristic. Which heuristic performs
   better, and why does that make sense given admissibility/informedness?
5. **(Analysis, 20 pts)** For a larger, harder puzzle instance, compare runtime and nodes
   expanded across BFS, DFS, and A* (best heuristic). Summarize your findings in a short table
   plus 3–5 sentences of discussion.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
