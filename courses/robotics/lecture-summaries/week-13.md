# Week 13 Summary — Path Planning I: Grid-Based Planning

**Key takeaways:**
- Configuration space represents all possible robot configurations; obstacles become forbidden
  regions; this course approximates C-space with the occupancy grid from Week 9.
- A* applied to a navigation grid is the same algorithm as classical graph search A*, with grid
  cells as states and adjacent free cells as actions.
- The heuristic (Manhattan/Euclidean distance on the grid) must stay admissible for the planned
  path to be optimal.

**You should now be able to:** implement A* on an occupancy grid; visualize a planned path;
compare heuristics' effect on the resulting path.

**Reminder:** Assignment 3 (path planning) assigned this week, due start of Week 15.
**Next week:** sampling-based planning (RRT) for when grid search doesn't scale well.
