# Lab Notes 13 — Grid-Based Path Planning (A*)

**Concept recap:** A* on a grid treats each free cell as a state and movement to an adjacent
free cell as an action; the heuristic must stay admissible for the planned path to remain
optimal.

**Common pitfalls:**
- Allowing diagonal moves without adjusting the step cost (should be `sqrt(2)` not `1` if
  diagonals are allowed) — easy to forget and silently biases the planner.
- Not checking grid bounds before indexing, causing an out-of-bounds error near grid edges.
- Reusing a `visited` set incorrectly across multiple planning calls (stale state from a
  previous run) — reset it at the start of each planning call.

**Debugging tip:** visualize the occupancy grid alone first, before adding the planned path
overlay — confirms the grid itself (free vs. occupied cells) is correct before debugging the
planner.

**Instructor tip:** Task C's heuristic comparison directly reinforces the classical-AI-style A*
lesson (heuristic choice affects nodes expanded, not just path quality) in a navigation-specific
context.
