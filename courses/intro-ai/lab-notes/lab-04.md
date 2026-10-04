# Lab Notes 4 — Informed Search: A*, Hill Climbing, Simulated Annealing

**Concept recap:** A* expands the node with lowest `f(n) = g(n) + h(n)`; an admissible heuristic
guarantees optimality. Hill climbing moves greedily to the best neighbor and stops at any local
optimum; simulated annealing occasionally accepts worse moves (with probability shrinking as
"temperature" cools) to escape local optima.

**Common pitfalls:**
- Using a heuristic that overestimates (inadmissible) without realizing it — always sanity-check
  `h(n) <= true_cost(n)` on a few hand-computed examples before trusting A*'s output.
- Forgetting to re-check/update `best_g` when a state is reached again via a cheaper path in
  graph-search A* — without this, A* can return a suboptimal path.
- In simulated annealing, cooling the temperature too quickly (it behaves like plain hill
  climbing) or too slowly (it wastes the step budget on near-random moves); plot the acceptance
  rate over time if results look wrong.

**Debugging tip:** log `(state, g, h, f)` for every node popped from A*'s frontier; the `f`
values should be non-decreasing in the order popped once a state has been finalized — if not,
the heuristic is likely inconsistent.

**Instructor tip:** run simulated annealing many times (e.g., 20 trials) from the same bad
starting point and show the distribution of final results — a single run can look like "it
worked" or "it didn't" by chance, and the point of the algorithm is only visible in aggregate.
