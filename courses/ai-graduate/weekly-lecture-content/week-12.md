# Week 12 — Lecture Content: Computational Complexity of AI Problems

## 1. A Unifying Look Back
This week connects results introduced separately in Weeks 6, 7, and 11 into one coherent
complexity-theoretic picture of why AI problems are hard, and why that does not make AI hopeless.

## 2. NP-Completeness of SAT
**The Cook-Levin theorem (stated).** Boolean satisfiability (SAT) was the first problem proven
NP-complete (Cook, 1971; independently Levin). "NP-complete" means two things simultaneously:
(1) SAT is in NP — a candidate satisfying assignment can be *verified* in polynomial time (just
plug in the values and check every clause); and (2) SAT is NP-hard — every problem in NP can be
reduced to SAT in polynomial time, so SAT is at least as hard as any problem whose solutions are
polynomial-time verifiable. Because so many practical problems reduce to SAT (or have a SAT-like
structure), this makes SAT the canonical "hardest problem we can efficiently verify" benchmark.

## 3. NP-Completeness of CSP
General constraint satisfaction is also NP-complete: membership in NP is immediate (a candidate
assignment can be checked against all constraints in polynomial time); NP-hardness follows by
reduction — for instance, 3-SAT (SAT restricted to clauses of exactly 3 literals, itself
NP-complete) can be encoded directly as a CSP where each clause becomes a constraint over its
three variables. This is why, despite decades of CSP algorithm engineering (arc consistency,
ordering heuristics, Week 5), no general CSP algorithm runs in worst-case polynomial time — such
an algorithm would imply P = NP.

## 4. PSPACE-Completeness of Planning, Revisited
Week 7 introduced the result that general STRIPS plan existence is PSPACE-complete. Recall the
two-part intuition: membership in PSPACE because a plan can be checked incrementally in
polynomial space (never needing to store the whole plan or all reachable states at once), and
PSPACE-hardness because planning can simulate a polynomial-space-bounded Turing machine's
computation. The relationship **NP ⊆ PSPACE** holds because any nondeterministic polynomial-time
computation is trivially also a polynomial-space computation (it cannot use more space than the
polynomial time it runs for allows it to write), so every NP problem is also in PSPACE. Whether
this containment is *strict* (NP ⊊ PSPACE) is, like P vs. NP, a famous open problem in complexity
theory — but it is widely believed to be strict, which is the formal grounding for the informal
claim "planning is harder than SAT."

## 5. Why This Does Not Make AI Hopeless
These are **worst-case** results: they say that no algorithm can solve *every* instance of SAT,
CSP, or planning efficiently (assuming P ≠ NP and NP ⊊ PSPACE). They do **not** say every
instance is hard. Three concrete reasons practical AI systems cope with this:
1. **Heuristics.** A*/IDA* with a good heuristic, or a relaxed-planning-graph heuristic (Week 7),
   can make most *practically encountered* instances fast, even though pathological worst-case
   instances remain hard.
2. **Approximation and incompleteness.** Sampling-based inference (Week 11) and local-search
   metaheuristics (Week 5) trade a worst-case guarantee for good-enough answers most of the time.
3. **Structure exploitation.** Restricted problem classes (e.g., low-treewidth graphical models,
   Horn-clause SAT instances, HTN-structured planning domains) admit genuinely polynomial
   algorithms — the worst-case hardness result applies to the *general* problem, not to every
   structurally restricted subclass of it.

## 6. Empirical Demonstration: Runtime Scaling
```python
import time, random

def random_3sat(n_vars, n_clauses):
    clauses = []
    for _ in range(n_clauses):
        vars_chosen = random.sample(range(1, n_vars + 1), 3)
        clause = frozenset(v if random.random() < 0.5 else -v for v in vars_chosen)
        clauses.append(clause)
    return clauses

def time_dpll(dpll_fn, n_vars, clause_to_var_ratio, trials=5):
    n_clauses = int(n_vars * clause_to_var_ratio)
    total_time = 0.0
    for _ in range(trials):
        clauses = random_3sat(n_vars, n_clauses)
        start = time.perf_counter()
        dpll_fn(clauses)
        total_time += time.perf_counter() - start
    return total_time / trials

# Random 3-SAT is known to be empirically hardest near the clause/variable ratio ~4.3
# ("the satisfiability threshold"); instances well below or above this ratio tend to be
# much easier for a DPLL-style solver, which is itself a striking, real empirical phenomenon
# connecting average-case difficulty to the clause/variable ratio, distinct from worst-case
# NP-completeness.
```
(This reuses the `dpll` implementation from Week 6.) Running this sweep across several ratios
typically shows a clear spike in average runtime near the satisfiability threshold, and much
faster solving on either side of it — a concrete, empirical illustration that "NP-complete" is a
worst-case statement, not a claim that every instance is equally hard.

## 7. In-Class/Lab Exercise
Sweep the clause-to-variable ratio from 2.0 to 6.0 in steps of 0.5, run `time_dpll` at each
ratio for a fixed number of variables (e.g., 20), plot average runtime vs. ratio, and identify
the empirical difficulty spike near the satisfiability threshold.
