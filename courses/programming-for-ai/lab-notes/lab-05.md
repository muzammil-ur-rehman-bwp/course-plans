# Lab Notes 5 — BFS & DFS

**Concept recap:** a `Problem` class packages `actions`/`result`/`is_goal`; BFS uses a FIFO
queue (`deque`) and is optimal for unweighted graphs; DFS uses a stack/recursion and is not
optimal but uses less memory.

**Common pitfalls:**
- Forgetting the `visited` set, causing infinite loops on mazes with cycles/open loops.
- Off-by-one errors when indexing the grid (`grid[row][col]` vs. `grid[col][row]` — be
  consistent about (row, col) vs. (x, y) throughout).
- Appending to the wrong end of the `deque` (use `popleft()` for BFS, not `pop()`).

**Debugging tip:** print the frontier size at each step to sanity-check whether BFS is exploring
breadth-first as expected (it should grow roughly level-by-level, not run deep before wide).

**Instructor tip:** have students visualize the explored/visited cells overlaid on the maze —
seeing BFS fill outward like a wave vs. DFS diving into one branch makes the algorithmic
difference intuitive, not just a complexity-table abstraction.
