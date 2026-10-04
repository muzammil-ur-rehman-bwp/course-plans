# Week 6 Summary — Informed Search: Greedy & A*

**Key takeaways:**
- A heuristic `h(n)` must be admissible (never overestimates) for A* to guarantee optimality;
  consistency is a stronger property that also avoids re-expansion.
- Greedy Best-First Search expands by `h(n)` alone — fast, not optimal.
- A* expands by `f(n) = g(n) + h(n)` — optimal with an admissible heuristic, and in practice
  expands far fewer nodes than BFS.
- `heapq` gives an efficient priority-queue implementation for both algorithms.

**You should now be able to:** design and justify a heuristic; implement Greedy/A* with
`heapq`; compare search strategies by nodes expanded and path optimality.

**Reminder:** Assignment 2 (search algorithms) is assigned this week, due start of Week 8.
**Next week:** Constraint Satisfaction Problems and local search.
