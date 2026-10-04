# Lab Notes 3 — Uninformed Search: BFS, DFS, UCS

**Concept recap:** a `Problem` class packages `actions`/`result`/`is_goal`/`step_cost`; BFS uses
a FIFO queue and is optimal for unweighted graphs; DFS uses a stack/recursion and is not optimal
but uses less memory; UCS generalizes BFS to weighted graphs via a priority queue ordered by
cumulative cost.

**Common pitfalls:**
- Forgetting the `visited` (or `best_cost`) set/dict, causing infinite loops on graphs with
  cycles.
- Appending to the wrong end of the `deque` (use `popleft()` for BFS, not `pop()`).
- In UCS, comparing heap tuples that contain unorderable items (e.g., raw state objects) when
  costs tie — always include a tie-breaking counter (as shown in lecture) so the heap never
  needs to compare states directly.
- Off-by-one errors when indexing a grid (`grid[row][col]` vs. `grid[col][row]`) — be consistent
  about (row, col) vs. (x, y) throughout.

**Debugging tip:** print the frontier size at each step to sanity-check whether BFS is exploring
breadth-first as expected (it should grow roughly level-by-level, not run deep before wide); for
UCS, print the cost popped at each iteration and confirm it is non-decreasing.

**Instructor tip:** have students visualize the explored/visited cells overlaid on the maze —
seeing BFS fill outward like a wave vs. DFS diving into one branch makes the algorithmic
difference intuitive, not just a complexity-table abstraction.
