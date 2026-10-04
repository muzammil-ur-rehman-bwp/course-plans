# Week 14 Summary — Path Planning II: Sampling-Based Planning

**Key takeaways:**
- Grid-based search doesn't scale well to high-dimensional configuration spaces; sampling-based
  methods trade completeness for scalability.
- RRT grows a tree via random sampling + nearest-node extension, finding a valid (not
  necessarily optimal) path quickly even in large/continuous spaces.
- Potential-field-style reactive local avoidance is simple and fast but can get stuck at local
  minima — directly analogous to local search's local-optima problem from classical AI.
- A* gives shorter/optimal grid paths; RRT scales better and runs faster in harder spaces — the
  right choice depends on the planning problem's dimensionality and real-time constraints.

**You should now be able to:** implement a basic RRT planner; explain when sampling-based
planning is preferable to grid search.

**Next week:** integrating perception, estimation, planning, and control into one navigation
pipeline.
