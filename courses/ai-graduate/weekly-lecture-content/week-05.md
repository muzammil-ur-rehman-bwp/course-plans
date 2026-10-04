# Week 5 — Lecture Content: Rigorous CSP & Combinatorial Optimization

## 1. Advanced CSP: Arc Consistency (AC-3)
A **constraint satisfaction problem (CSP)** is ⟨X, D, C⟩: variables X, domains D, and constraints
C. **Arc consistency**: an arc (variable Xᵢ, variable Xⱼ) is consistent if, for every value in
Xᵢ's domain, there is some value in Xⱼ's domain satisfying the constraint between them. AC-3
repeatedly removes values that violate arc consistency until a fixed point:

```python
from collections import deque

def ac3(domains, constraints, neighbors):
    """domains: dict var -> list of values. constraints: dict (Xi,Xj) -> function(x, y) -> bool.
    neighbors: dict var -> list of vars sharing a constraint. Mutates domains in place."""
    queue = deque((xi, xj) for xi in domains for xj in neighbors[xi])
    while queue:
        xi, xj = queue.popleft()
        if revise(domains, constraints, xi, xj):
            if not domains[xi]:
                return False  # domain wiped out -> no solution
            for xk in neighbors[xi]:
                if xk != xj:
                    queue.append((xk, xi))
    return True

def revise(domains, constraints, xi, xj):
    revised = False
    constraint = constraints.get((xi, xj)) or constraints.get((xj, xi))
    for x in list(domains[xi]):
        if not any(constraint(x, y) for y in domains[xj]):
            domains[xi].remove(x)
            revised = True
    return revised
```
AC-3 runs in O(cd³) worst-case time for c constraints and domain size d, and is typically used as
a preprocessing/pruning step before or during backtracking search, not as a standalone solver —
it establishes arc consistency, which is necessary but not sufficient for a global solution.

**Ordering heuristics for backtracking.** Minimum-remaining-values (MRV) selects the variable
with the fewest legal values left (fail fast); least-constraining-value (LCV) orders a variable's
value choices to rule out the fewest options for neighboring variables. Combined with forward
checking (propagating each assignment's immediate constraint consequences), these heuristics
substantially reduce the backtracking search tree in practice, though worst-case complexity
remains exponential (CSP is NP-complete in general — formalized in Week 12).

## 2. Simulated Annealing
Simulated annealing searches a space of complete candidate solutions (not partial assignments),
accepting moves that improve the objective and, crucially, *sometimes* accepting moves that make
it worse — with a probability controlled by a temperature parameter T that decreases over time.

```python
import math, random

def simulated_annealing(initial_state, objective, neighbor, schedule, max_steps=10000):
    """objective: lower is better. schedule(t) -> temperature at step t (decreasing to ~0)."""
    current = initial_state
    current_cost = objective(current)
    for t in range(max_steps):
        T = schedule(t)
        if T <= 1e-12:
            break
        candidate = neighbor(current)
        delta = objective(candidate) - current_cost
        if delta < 0 or random.random() < math.exp(-delta / T):
            current, current_cost = candidate, objective(candidate)
    return current, current_cost
```

**The Metropolis acceptance criterion.** A worsening move with cost increase Δ > 0 is accepted
with probability exp(−Δ/T). At high T, nearly all moves are accepted (exploration); as T → 0,
only improving moves are accepted (exploitation) — this is a literal embedding of the
exploration-exploitation tradeoff (which reappears formally in Week 9's reinforcement learning)
into a local-search optimizer.

**Convergence discussion (conceptual).** Simulated annealing is known to converge in probability
to a global optimum *if* the cooling schedule decreases sufficiently slowly — specifically,
results in the annealing literature show convergence guarantees under logarithmic cooling
schedules (T(t) ∝ 1/log(t)), which are far too slow to be practical. In practice, faster
(e.g., geometric, T(t) = T₀·αᵗ) cooling schedules are used, which sacrifice the theoretical
guarantee for a solution that is usually good, not provably optimal, in reasonable time. This is
a genuine, important gap between the convergence theory and practical use that is worth stating
explicitly at graduate level, rather than leaving the convergence claim as folklore.

## 3. Genetic Algorithms
Genetic algorithms maintain a *population* of candidate solutions and apply biologically inspired
operators: **selection** (fitter individuals are more likely to be chosen as parents),
**crossover** (combining two parents' representations to produce offspring), and **mutation**
(random perturbation, maintaining diversity).

```python
import random

def genetic_algorithm(population, fitness, crossover, mutate, generations=100, elite=2):
    for _ in range(generations):
        population = sorted(population, key=fitness, reverse=True)
        next_gen = population[:elite]  # elitism: keep the best individuals
        while len(next_gen) < len(population):
            p1, p2 = random.choices(population[:len(population)//2], k=2)
            child = mutate(crossover(p1, p2))
            next_gen.append(child)
        population = next_gen
    return max(population, key=fitness)
```

**Why genetic algorithms have no convergence guarantee.** Unlike simulated annealing's
(impractical-but-real) asymptotic convergence result under a specific cooling schedule, there is
no general theorem guaranteeing a genetic algorithm converges to the global optimum under
reasonable conditions. Genetic algorithms are a heuristic search strategy over a population; they
can suffer **premature convergence** (the population loses diversity and gets stuck in a local
optimum) with no built-in mechanism (analogous to annealing's temperature schedule) that
provably escapes this in the limit. This is an important, honest distinction to draw at graduate
level: simulated annealing has an asymptotic theoretical guarantee (even if impractical);
genetic algorithms, in general, do not.

## 4. In-Class/Lab Exercise
Implement simulated annealing for a small (10–15 city) traveling-salesperson instance; plot tour
cost vs. iteration for a geometric cooling schedule, and discuss what changing the cooling rate
does to solution quality and runtime.
