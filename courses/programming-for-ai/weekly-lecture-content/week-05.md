# Week 5 — Lecture Content: Problem Solving as Search

## 1. Formulating a Search Problem
A search problem is defined by:
- **State space**: the set of all possible configurations.
- **Initial state**: where the agent starts.
- **Actions**: `actions(state)` returns the set of actions applicable in a state.
- **Transition model**: `result(state, action)` returns the resulting state.
- **Goal test**: `is_goal(state)` returns True if `state` is a goal.
- **Path cost**: a function assigning a numeric cost to a path (often just the number of steps).

```python
class Problem:
    def __init__(self, initial, goal):
        self.initial = initial
        self.goal = goal

    def actions(self, state):
        raise NotImplementedError

    def result(self, state, action):
        raise NotImplementedError

    def is_goal(self, state):
        return state == self.goal
```

## 2. Breadth-First Search (BFS)
Explores the state space level by level using a FIFO queue — guarantees the shortest path (in
number of steps) for unweighted graphs.
```python
from collections import deque

def bfs(problem):
    frontier = deque([(problem.initial, [])])
    visited = {problem.initial}
    while frontier:
        state, path = frontier.popleft()
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            if next_state not in visited:
                visited.add(next_state)
                frontier.append((next_state, path + [action]))
    return None
```

## 3. Depth-First Search (DFS)
Explores as deep as possible along a branch before backtracking, using a LIFO stack (or
recursion). Uses less memory than BFS but does not guarantee the shortest path.
```python
def dfs(problem):
    frontier = [(problem.initial, [])]
    visited = {problem.initial}
    while frontier:
        state, path = frontier.pop()
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            if next_state not in visited:
                visited.add(next_state)
                frontier.append((next_state, path + [action]))
    return None
```

## 4. Comparison
| Property | BFS | DFS |
|---|---|---|
| Complete (finds a solution if one exists) | Yes (finite state space) | Yes (finite state space, with visited set) |
| Optimal (fewest steps) | Yes (unweighted) | No |
| Time complexity | O(b^d) | O(b^m) |
| Space complexity | O(b^d) (high) | O(bm) (low) |
*(b = branching factor, d = depth of shallowest goal, m = max depth)*

## 5. Worked Example
Apply both `bfs()` and `dfs()` to a maze represented as a 2D grid, reusing the `Graph` class
pattern introduced in Week 2.

## 6. In-Class Exercise
Trace BFS and DFS by hand on a 5-node graph, then confirm the result by running the provided
implementations.
