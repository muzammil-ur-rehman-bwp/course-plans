# Week 7 Summary — Rigorous Classical Planning

**Key takeaways:**
- Plan existence for general STRIPS planning is PSPACE-complete: a nondeterministic
  polynomial-space guess-and-check procedure (plus Savitch's theorem) gives membership; encoding
  polynomial-space Turing machine computations into planning gives hardness.
- The planning graph alternates literal and action layers built forward from the initial state;
  ignoring delete lists (the relaxed problem) gives an efficiently computable, informative
  heuristic (h_level) widely used in practical heuristic-search planners.
- HTN planning decomposes abstract tasks into subtasks via domain-authored methods, exploiting
  structure that flat STRIPS search ignores, at the cost of requiring a hand-built task
  hierarchy.

**You should now be able to:** state and give intuition for planning's PSPACE-completeness;
build a planning graph and compute a relaxed-plan heuristic value; implement a simple HTN
decomposition.

**Next week:** Markov Decision Processes I — the MDP formalism, the Bellman equation, and value
iteration, plus a review session ahead of the midterm.
