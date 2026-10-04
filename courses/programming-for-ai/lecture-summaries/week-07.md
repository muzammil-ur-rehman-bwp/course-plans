# Week 7 Summary — CSPs & Local Search

**Key takeaways:**
- A CSP is defined by variables, domains, and constraints (e.g., N-Queens, map coloring, Sudoku).
- Backtracking search explores assignments systematically; forward checking prunes future
  domains early to detect failure sooner.
- Hill climbing is a local search method that can get stuck at a local optimum.
- Simulated annealing escapes local optima by probabilistically accepting worse moves, with the
  acceptance probability shrinking as "temperature" decreases.

**You should now be able to:** formulate a problem as a CSP; implement backtracking with forward
checking; implement and tune hill climbing / simulated annealing for an optimization problem.

**Next week:** probability and uncertainty, leading into the midterm review.
