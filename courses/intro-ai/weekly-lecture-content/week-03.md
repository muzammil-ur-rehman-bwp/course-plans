# Week 3 — Lecture Content: Uninformed Search

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

    def step_cost(self, state, action, next_state):
        return 1
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

## 4. Uniform-Cost Search (UCS)
Like BFS, but the frontier is a priority queue ordered by cumulative path cost rather than
insertion order. Optimal whenever step costs are non-negative — this is exactly Dijkstra's
algorithm applied to an implicit graph.
```python
import heapq
import itertools

def uniform_cost_search(problem):
    counter = itertools.count()  # tie-breaker so heap never compares states directly
    frontier = [(0, next(counter), problem.initial, [])]
    best_cost = {problem.initial: 0}
    while frontier:
        cost, _, state, path = heapq.heappop(frontier)
        if problem.is_goal(state):
            return path, cost
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            new_cost = cost + problem.step_cost(state, action, next_state)
            if next_state not in best_cost or new_cost < best_cost[next_state]:
                best_cost[next_state] = new_cost
                heapq.heappush(frontier, (new_cost, next(counter), next_state, path + [action]))
    return None, float("inf")
```

## 5. Comparison
| Property | BFS | DFS | UCS |
|---|---|---|---|
| Complete (finds a solution if one exists) | Yes (finite space) | Yes (with visited set) | Yes (non-negative costs) |
| Optimal | Yes, if unweighted | No | Yes |
| Time complexity | O(b^d) | O(b^m) | O(b^(1+⌊C*/ε⌋)) |
| Space complexity | O(b^d) (high) | O(bm) (low) | Similar to BFS |
*(b = branching factor, d = depth of shallowest goal, m = max depth, C* = optimal cost, ε = minimum step cost)*

## 6. Worked Example
Consider a tiny graph `A-B (1), A-C (4), B-D (2), C-D (1)` with start `A` and goal `D`. BFS
(treating all edges as cost 1) returns the 2-step path `A-B-D`. UCS, honoring the weights,
compares path `A-B-D` (cost 3) against `A-C-D` (cost 5) and correctly also returns `A-B-D`, but
would differ from BFS whenever the fewest-edges path is not the cheapest one.

## 7. In-Class Exercise
Trace BFS, DFS, and UCS by hand on the 4-node weighted graph above, then confirm the results by
running the three implementations.
