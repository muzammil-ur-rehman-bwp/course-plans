# Week 4 — Lecture Content: Informed Search

## 1. Heuristics
A **heuristic function** `h(n)` estimates the cost from node `n` to the nearest goal, using
problem-specific knowledge. For grid/path problems, Manhattan distance (sum of absolute row/col
differences) is a common heuristic.

```python
def manhattan_distance(a, b):
    (r1, c1), (r2, c2) = a, b
    return abs(r1 - r2) + abs(c1 - c2)
```

**Admissibility**: `h(n)` never overestimates the true cost to the goal (`h(n) <= h*(n)`).
**Consistency** (monotonicity): for every edge `n -> n'` with cost `c`, `h(n) <= c + h(n')`.
Consistency implies admissibility, and is the condition that actually matters for A*'s
correctness under graph search.

## 2. Greedy Best-First Search
Expands the node with the lowest `h(n)`, ignoring the cost already paid to reach it. This can be
led astray by a path that looks good locally but is globally expensive — it is neither complete
nor optimal in general (though it is often fast in practice).

## 3. A* Search
Expands the node with the lowest `f(n) = g(n) + h(n)`, where `g(n)` is the cost so far and
`h(n)` is the heuristic estimate of the remaining cost. With an admissible heuristic (and
consistent, for graph search with re-expansion handling), A* is guaranteed to find an optimal
solution.

```python
import heapq
import itertools

def a_star(problem, heuristic):
    counter = itertools.count()
    start_h = heuristic(problem.initial, problem.goal)
    frontier = [(start_h, 0, next(counter), problem.initial, [])]
    best_g = {problem.initial: 0}
    while frontier:
        f, g, _, state, path = heapq.heappop(frontier)
        if problem.is_goal(state):
            return path, g
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            new_g = g + problem.step_cost(state, action, next_state)
            if next_state not in best_g or new_g < best_g[next_state]:
                best_g[next_state] = new_g
                new_f = new_g + heuristic(next_state, problem.goal)
                heapq.heappush(frontier, (new_f, new_g, next(counter), next_state, path + [action]))
    return None, float("inf")
```

**Why admissibility implies optimality (brief argument):** suppose A* returns a goal `G` with
cost `g(G)` but a cheaper goal `G*` exists with true cost `C* < g(G)`. Since `h` never
overestimates, every ancestor `n` of `G*` on the optimal path has `f(n) = g(n) + h(n) <= C*`.
So some ancestor of `G*` would have been expanded (or at least sit in the frontier with
`f <= C* < g(G)`) before A* could settle on the more expensive `G`, a contradiction.

## 4. Local Search: Hill Climbing
Starts from a candidate solution and repeatedly moves to the best neighboring state, stopping
when no neighbor improves on the current state. Simple and memory-efficient, but can get stuck
at a **local maximum**, a **plateau**, or a **ridge**.

```python
import random

def hill_climbing(objective, neighbors, start, max_steps=1000):
    current = start
    for _ in range(max_steps):
        candidates = neighbors(current)
        best = max(candidates, key=objective, default=None)
        if best is None or objective(best) <= objective(current):
            return current  # no improving neighbor: local optimum reached
        current = best
    return current
```

## 5. Local Search: Simulated Annealing
Like hill climbing, but occasionally accepts a *worse* move, with a probability that decreases
over time (controlled by a cooling "temperature"). This lets it escape local optima that pure
hill climbing cannot.

```python
import math
import random

def simulated_annealing(objective, neighbors, start, initial_temp=10.0, cooling=0.98, steps=500):
    current = start
    temp = initial_temp
    for _ in range(steps):
        candidates = neighbors(current)
        if not candidates:
            break
        next_state = random.choice(candidates)
        delta = objective(next_state) - objective(current)
        if delta > 0 or random.random() < math.exp(delta / max(temp, 1e-9)):
            current = next_state
        temp *= cooling
    return current
```

## 6. Worked Example
On a 5x5 grid with one wall segment, compute `f(n) = g(n) + h(n)` (Manhattan distance heuristic)
for each frontier node by hand and confirm A* expands nodes in the same order the code produces.

## 7. In-Class Exercise
Given the grid above, trace A* step by step; then discuss what happens if an *inadmissible*
heuristic (one that overestimates) is used instead — show a small example where it returns a
suboptimal path.
