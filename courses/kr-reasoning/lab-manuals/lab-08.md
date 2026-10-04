# Lab Manual 8 — AC-3 and Heuristic Backtracking Search

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement AC-3 and backtracking search with MRV and least-constraining-value heuristics; apply
both to a map-coloring and a small scheduling CSP.

## Setup
Create `lab08.ipynb`.

## Procedure
1. **Task A — AC-3:** implement `revise` and `ac3` from the lecture content; run it on a
   4-region map-coloring CSP (graph provided) with 3 colors, and report which values (if any) are
   pruned from which domains.
2. **Task B — Backtracking with heuristics:** implement `select_unassigned_variable` (MRV +
   degree), `order_domain_values` (LCV), and `backtracking_search`; solve the same map-coloring
   CSP and report the assignment found.
3. **Task C — Unsatisfiable case:** construct a small CSP that AC-3 reports as arc consistent but
   that has no actual solution (as in the lecture's worked example); run your `backtracking_search`
   on it and confirm it correctly returns `None`.
4. **Task D — Scheduling CSP:** formulate a small scheduling problem (e.g., 4 exams, 3 time
   slots, a few "cannot be at the same time" constraints) as a CSP and solve it with your
   `backtracking_search`.

## Expected Output
A notebook with Tasks A–D; working AC-3 and backtracking implementations, a correctly-identified
unsatisfiable case despite arc consistency, and a solved scheduling CSP.

## Submission
Submit `lab08.ipynb` by the end of the lab session.
