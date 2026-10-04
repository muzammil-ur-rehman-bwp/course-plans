# Week 3 Summary — Uninformed Search

**Key takeaways:**
- Any search problem can be formalized as: state space, initial state, actions, transition
  model, goal test, path cost.
- BFS (FIFO queue) is complete and optimal for unweighted graphs but memory-heavy (O(b^d)).
- DFS (stack/recursion) is memory-efficient (O(bm)) but not optimal.
- Uniform-cost search generalizes BFS to weighted graphs (it is Dijkstra's algorithm on an
  implicit graph) and is optimal whenever step costs are non-negative.
- All three rely on a `visited`/`best-cost` structure to avoid revisiting states.

**You should now be able to:** formalize a problem as a `Problem` class; implement and trace
BFS/DFS/UCS; explain their completeness/optimality/complexity tradeoffs.

**Next week:** informed search — using heuristics (A*) and local search (hill climbing,
simulated annealing) to search more effectively than uninformed methods.
