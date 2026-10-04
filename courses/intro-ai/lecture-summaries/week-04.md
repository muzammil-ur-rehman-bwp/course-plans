# Week 4 Summary — Informed Search

**Key takeaways:**
- A heuristic `h(n)` estimates remaining cost to the goal; admissibility (never overestimates)
  and consistency are the key correctness conditions.
- Greedy best-first search uses only `h(n)` and can be led astray; A* uses `f(n) = g(n) + h(n)`
  and is optimal with an admissible heuristic.
- Hill climbing and simulated annealing are local-search methods for optimization problems where
  the full state-space search of A* is unnecessary or infeasible; simulated annealing's
  temperature schedule lets it escape local optima that hill climbing cannot.

**You should now be able to:** check whether a heuristic is admissible; implement A* with
`heapq`; implement hill climbing and simulated annealing for a toy optimization problem.

**Next week:** adversarial search — minimax and alpha-beta pruning for two-player games.
