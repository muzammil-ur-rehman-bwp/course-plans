# Week 6 — Lecture Content: Informed Search — Greedy & A*

## 1. Heuristics
A heuristic `h(n)` estimates the cost from node `n` to the goal. Properties that matter:
- **Admissible**: `h(n)` never overestimates the true cost to the goal — required for A*
  optimality.
- **Consistent** (monotonic): `h(n) <= cost(n, n') + h(n')` for every neighbor `n'` — implies
  admissibility and lets A* avoid re-expanding nodes.

Common example: Manhattan distance or Euclidean distance as `h(n)` on a grid.

## 2. Greedy Best-First Search
Expands the node with the lowest `h(n)` first. Fast but not optimal — it can be led astray by a
locally attractive but globally poor heuristic value.
```python
import heapq

def greedy_best_first(problem, h):
    frontier = [(h(problem.initial), problem.initial, [])]
    visited = {problem.initial}
    while frontier:
        _, state, path = heapq.heappop(frontier)
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            if next_state not in visited:
                visited.add(next_state)
                heapq.heappush(frontier, (h(next_state), next_state, path + [action]))
    return None
```

## 3. A* Search
Expands the node with the lowest `f(n) = g(n) + h(n)`, where `g(n)` is the cost so far and `h(n)`
is the heuristic estimate to the goal. With an admissible heuristic, A* is guaranteed optimal.
```python
def a_star(problem, h, step_cost=lambda s, a: 1):
    frontier = [(h(problem.initial), 0, problem.initial, [])]
    best_g = {problem.initial: 0}
    while frontier:
        f, g, state, path = heapq.heappop(frontier)
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            new_g = g + step_cost(state, action)
            if next_state not in best_g or new_g < best_g[next_state]:
                best_g[next_state] = new_g
                heapq.heappush(frontier, (new_g + h(next_state), new_g, next_state, path + [action]))
    return None
```

## 4. Comparing Strategies
On the same grid-world problem, we instrument BFS, Greedy, and A* to count nodes expanded.
Typical finding: A* expands far fewer nodes than BFS while still finding the optimal path;
Greedy expands the fewest but may return a suboptimal (longer) path.

## 5. In-Class Exercise
For a given grid with obstacles, propose a heuristic, argue whether it's admissible, and run A*
with it; compare the resulting path length and nodes-expanded count against BFS.
