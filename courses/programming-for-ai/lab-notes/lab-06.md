# Lab Notes 6 — A* Search

**Concept recap:** `f(n) = g(n) + h(n)`; A* is optimal when `h` is admissible; `heapq` gives an
efficient min-priority-queue implementation keyed on `f(n)`.

**Common pitfalls:**
- Pushing tuples into `heapq` where the second element is a non-comparable object (e.g., a list)
  — if two `f` values tie, Python will try to compare the next tuple element, which can error or
  silently misbehave. Use a tie-breaking counter or make sure the second element is always
  comparable (state id, etc.).
- Using a heuristic that occasionally overestimates (inadmissible), which silently breaks A*'s
  optimality guarantee without raising any error.
- Recomputing `h(n)` repeatedly instead of caching it, which is a performance (not
  correctness) issue worth noting for larger search spaces.

**Debugging tip:** log `(f, g, h, state)` for every popped node to verify the priority queue is
actually expanding nodes in non-decreasing `f` order.

**Instructor tip:** have students deliberately test an inadmissible heuristic (e.g., multiply
Manhattan distance by 2) and observe that A* returns a suboptimal path — the failure mode makes
the admissibility requirement concrete.
