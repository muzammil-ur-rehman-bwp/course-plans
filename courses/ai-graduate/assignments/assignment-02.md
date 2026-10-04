# Assignment 2 — CSP/Optimization, Automated Reasoning, Planning (Weeks 5–8)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 8 |
**Due:** Start of Week 10

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(AC-3 and backtracking, 20 pts)** For a provided graph-coloring CSP instance, implement
   AC-3 as a preprocessing step, then heuristic backtracking search (MRV + forward checking).
   Report domains after AC-3 and the number of backtracks with and without the MRV heuristic.
2. **(Simulated annealing, 20 pts)** Implement simulated annealing for a provided 15-city TSP
   instance with a geometric cooling schedule. Run it 10 times with different random seeds and
   report the mean and standard deviation of the final tour cost. In 2–3 sentences, relate this
   variance to the Week 13-style point that a single run is not representative (you may
   anticipate this point now; it is formalized later in the course).
3. **(DPLL, 25 pts)** Implement DPLL with unit propagation for a provided set of 5 CNF formulas
   (a mix of satisfiable and unsatisfiable). Report SAT/UNSAT for each and, for one
   unsatisfiable instance, trace (in writing) the sequence of unit propagations and branches
   that leads DPLL to detect the conflict.
4. **(Planning complexity and heuristics, 20 pts)** For a provided small logistics STRIPS
   domain, build the relaxed planning graph and compute h_level for the goal from two different
   initial states. In 2–3 sentences, state the PSPACE-completeness result for general planning
   and explain why a cheap, inadmissible-in-the-worst-case-but-practically-useful heuristic like
   h_level is still valuable despite that worst-case hardness.
5. **(HTN decomposition, 15 pts)** Implement an HTN decomposition for a provided abstract task
   with at least two alternative methods, and show your implementation correctly selects a
   method based on precondition satisfaction.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
