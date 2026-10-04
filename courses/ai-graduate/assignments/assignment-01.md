# Assignment 1 — Advanced Search & Game Theory (Weeks 1–4)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 4 |
**Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(IDA*, 20 pts)** Implement IDA* for the 8-puzzle using the Manhattan-distance heuristic.
   Run it on 3 instances of increasing solution depth (provided in `assignment01_instances.json`)
   and report, for each: solution length, number of contour iterations, and wall-clock time.
   Compare peak memory behavior (qualitatively) to what plain A* would require, citing the
   Week 2 complexity analysis.
2. **(Bidirectional search, 15 pts)** Implement bidirectional BFS for an unweighted maze graph
   (provided as an adjacency list). Verify it finds a shortest path matching plain BFS, and
   report the number of nodes expanded by each.
3. **(Alpha-beta correctness, 20 pts)** Implement alpha-beta pruning for a small depth-limited
   game tree (provided as a nested dictionary). Confirm its returned value matches plain
   minimax's value on the same tree, and report the number of nodes visited by each. In 3–5
   sentences, explain — referencing the Week 3 correctness argument — why a cutoff never changes
   the value returned.
4. **(Nash equilibrium, 20 pts)** For 3 given 2x2 normal-form games (payoff matrices provided),
   compute all pure-strategy Nash equilibria using `find_pure_nash_equilibria` (or your own
   correct implementation). For the game whose equilibrium is not the jointly best outcome,
   explain in 2–3 sentences why it is still a Nash equilibrium.
5. **(Mechanism design reflection, 25 pts)** Implement a second-price auction simulation for 5
   bidders with given valuations. Then, sweep one bidder's bid above and below their true
   valuation (holding others fixed) and plot their resulting surplus, confirming the surplus is
   maximized at bid = true valuation. Write a 4–6 sentence summary connecting this result to why
   second-price mechanisms are used in real resource-allocation systems.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
