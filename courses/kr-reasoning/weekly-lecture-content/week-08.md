# Week 8 — Lecture Content: Constraint Satisfaction in Depth

## 1. CSP Formulation, Recapped
A **constraint satisfaction problem (CSP)** is defined by variables `X1, ..., Xn`, each with a
domain `Di` of possible values, and constraints restricting which combinations of values are
allowed. The **constraint graph** has a node per variable and an edge between any two variables
sharing a constraint — this structure is exactly what AC-3 and backtracking exploit below.

## 2. Arc Consistency and AC-3, In Depth
An arc `(Xi, Xj)` is **arc consistent** if for every value `x` in `Di`, there is *some* value `y`
in `Dj` consistent with `x` under the constraint between them. If not, any such unsupported `x`
can be safely removed from `Di` — it could never be part of a solution. **AC-3** repeatedly
revises arcs from a worklist until no arc has anything left to revise (a fixed point) or some
domain becomes empty (the CSP is unsatisfiable).

```python
from collections import deque

def revise(domains, xi, xj, constraint):
    """Remove values from domains[xi] that have no supporting value in domains[xj]. Returns
    True if domains[xi] was changed."""
    revised = False
    for x in list(domains[xi]):
        if not any(constraint(x, y) for y in domains[xj]):
            domains[xi].remove(x)
            revised = True
    return revised

def ac3(variables, domains, neighbors, constraint):
    """neighbors: var -> set of vars sharing a constraint with it."""
    worklist = deque((xi, xj) for xi in variables for xj in neighbors[xi])
    while worklist:
        xi, xj = worklist.popleft()
        if revise(domains, xi, xj, constraint):
            if not domains[xi]:
                return False  # a domain went empty: no solution possible
            for xk in neighbors[xi] - {xj}:
                worklist.append((xk, xi))  # xk's arc to xi may now need re-revising
    return True
```

**What AC-3 guarantees:** every remaining value in every domain has *some* support in every
neighboring domain. **What it does not guarantee:** that a full solution exists — arc consistency
is a *local* (pairwise) consistency check; a CSP can be arc consistent yet still have no global
solution (this is why backtracking search, below, is still needed in general).

### Worked Trace
3 variables `A, B, C`, domains `{1, 2}` each, constraint `≠` between every pair (like a tiny
map-coloring instance with only 2 colors and all three regions mutually adjacent — unsatisfiable).
AC-3 processes `(A, B)`: every value in `A`'s domain has a supporting value in `B` (1 supports 2,
2 supports 1), so no revision. The same holds for every other arc — AC-3 reports arc consistent,
*but no 3-coloring with 2 colors and all-pairs adjacency actually exists*; this is exactly the
gap mentioned above, and backtracking search (next) is what actually finds (or correctly fails to
find) a full assignment.

## 3. Backtracking Search with Ordering Heuristics
Plain backtracking tries values for variables one at a time, undoing a choice when it leads to a
dead end. Two heuristics make this dramatically faster in practice without changing correctness:

- **Minimum-remaining-values (MRV):** choose next the variable with the *fewest* legal values
  left — it is most likely to fail soon, so failing fast here prunes the search tree earlier.
- **Degree heuristic:** among ties, prefer the variable involved in the most constraints with
  other unassigned variables (a useful tie-breaker, and a reasonable first choice before any
  values have been assigned at all).
- **Least-constraining-value (LCV):** for the chosen variable, try first the value that rules out
  the *fewest* choices for neighboring variables — it is least likely to lead to failure.

```python
def select_unassigned_variable(assignment, variables, domains, neighbors):
    unassigned = [v for v in variables if v not in assignment]
    return min(unassigned, key=lambda v: (len(domains[v]), -len(neighbors[v])))  # MRV, then degree

def order_domain_values(var, assignment, domains, neighbors, constraint):
    def count_conflicts(value):
        count = 0
        for neighbor in neighbors[var]:
            if neighbor not in assignment:
                count += sum(1 for nv in domains[neighbor] if not constraint(value, nv))
        return count
    return sorted(domains[var], key=count_conflicts)  # least-constraining-value first

def backtracking_search(variables, domains, neighbors, constraint, assignment=None):
    if assignment is None:
        assignment = {}
    if len(assignment) == len(variables):
        return assignment
    var = select_unassigned_variable(assignment, variables, domains, neighbors)
    for value in order_domain_values(var, assignment, domains, neighbors, constraint):
        if all(constraint(value, assignment[n]) for n in neighbors[var] if n in assignment):
            assignment[var] = value
            result = backtracking_search(variables, domains, neighbors, constraint, assignment)
            if result is not None:
                return result
            del assignment[var]
    return None
```

**Forward checking** (combining the two ideas) removes, after each assignment, any neighboring
value now inconsistent with it — a lightweight, incremental form of the same propagation idea
behind AC-3, run *during* search rather than as a separate pre-processing pass.

## 4. In-Class Exercise
For a 4-region map-coloring CSP with 3 colors (graph and adjacency provided), run AC-3 by hand
first; then, on the (still only locally consistent) domains it leaves, apply MRV to pick the first
backtracking variable and LCV to order its values.

## 5. Midterm Review Roadmap
The midterm (Week 9) covers Weeks 1–8: KR desiderata (Wk 1); propositional logic, normal forms,
resolution, SAT (Wk 2); FOL syntax/semantics/translation (Wk 3); unification, FOL resolution,
Skolemization (Wk 4); production systems, forward/backward chaining (Wk 5); semantic
networks/frames, inheritance and exceptions (Wk 6); description logics, DL/FOL, OWL/RDF (Wk 7);
CSP formulation, AC-3, backtracking heuristics (Wk 8, this week). Practice problems covering one
derivation/trace from each week will be distributed separately.
