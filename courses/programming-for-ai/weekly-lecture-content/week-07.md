# Week 7 — Lecture Content: CSPs & Local Search

## 1. Constraint Satisfaction Problem (CSP) Formulation
A CSP consists of:
- **Variables**: `X1, ..., Xn`
- **Domains**: `D1, ..., Dn` — possible values for each variable
- **Constraints**: restrictions on allowed combinations of values

Example — Map coloring: variables = regions, domains = {red, green, blue}, constraint = adjacent
regions must differ in color. Example — N-Queens: variables = queen per column, domain = row
index, constraint = no two queens share a row/diagonal.

## 2. Backtracking Search with Forward Checking
```python
def backtracking(assignment, variables, domains, constraints):
    if len(assignment) == len(variables):
        return assignment
    var = next(v for v in variables if v not in assignment)
    for value in domains[var]:
        assignment[var] = value
        if constraints(assignment):
            result = backtracking(assignment, variables, domains, constraints)
            if result is not None:
                return result
        del assignment[var]
    return None
```
**Forward checking** prunes future domains immediately after each assignment, eliminating values
that would violate a constraint — this detects failure earlier than plain backtracking.

## 3. Local Search: Hill Climbing
Starts from a random complete assignment and repeatedly moves to a better neighboring state
(fewer constraint violations). Can get stuck in a **local optimum** — a state that is locally
best but not globally best.
```python
import random

def hill_climbing(initial_state, neighbors_fn, cost_fn, max_steps=1000):
    current = initial_state
    for _ in range(max_steps):
        neighbors = neighbors_fn(current)
        best_neighbor = min(neighbors, key=cost_fn)
        if cost_fn(best_neighbor) >= cost_fn(current):
            break  # local optimum reached
        current = best_neighbor
    return current
```

## 4. Simulated Annealing
Escapes local optima by occasionally accepting a worse move, with the probability of accepting a
worse move decreasing over time (controlled by a "temperature" schedule).
```python
import math

def simulated_annealing(initial_state, neighbors_fn, cost_fn, schedule):
    current = initial_state
    for t, temperature in enumerate(schedule):
        if temperature == 0:
            return current
        next_state = random.choice(neighbors_fn(current))
        delta = cost_fn(current) - cost_fn(next_state)  # positive = improvement
        if delta > 0 or random.random() < math.exp(delta / temperature):
            current = next_state
    return current
```

## 5. In-Class Exercise
Model N-Queens as a CSP, solve it with backtracking + forward checking, then solve the same
problem with simulated annealing; compare runtime and solution quality.
