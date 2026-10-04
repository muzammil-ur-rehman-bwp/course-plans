# Week 5 Summary — Problem Solving as Search

**Key takeaways:**
- Any search problem can be formalized as: state space, initial state, actions, transition
  model, goal test, path cost.
- BFS (FIFO queue) is complete and optimal for unweighted graphs but memory-heavy (O(b^d)).
- DFS (stack/recursion) is memory-efficient (O(bm)) but not optimal.
- Both algorithms rely on a `visited` set to avoid revisiting states (critical in graphs with
  cycles).

**You should now be able to:** formalize a problem as a `Problem` class; implement and trace
BFS/DFS; explain their complexity/optimality tradeoffs.

**Next week:** informed search (Greedy Best-First, A*) — using problem-specific knowledge
(heuristics) to search more efficiently than BFS/DFS.
