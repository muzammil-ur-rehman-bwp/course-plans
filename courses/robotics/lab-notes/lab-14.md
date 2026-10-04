# Lab Notes 14 — RRT Planning & Comparison with A*

**Concept recap:** RRT grows a tree via random sampling + nearest-node extension; it trades
path optimality for scalability to larger/continuous/higher-dimensional spaces compared to grid
search.

**Common pitfalls:**
- `obstacle_check` only checking the endpoint of a step, not the line segment between nodes —
  this can let the tree "tunnel through" thin obstacles.
- Using a `step_size` too large relative to obstacle size, causing the same tunneling issue even
  with correct endpoint checking.
- Not handling the case where RRT fails to reach the goal within `max_iter` (should return
  `None` or similar, not crash).

**Debugging tip:** visualize the full tree (all nodes and edges), not just the final path — a
healthy RRT run should show branches spreading across the free space, not clustering in one
area.

**Instructor tip:** Task D's parameter sensitivity experiment is where the "RRT trades
optimality for speed" lesson becomes concrete and measurable, not just a verbal claim.
