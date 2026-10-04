# Lab Manual 6 — Distribution Semantics Evaluator

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement a brute-force distribution-semantics evaluator and contrast it with an MLN-style
encoding of the same scenario.

## Setup
1. Reuse your Week 1 environment.
2. Create `lab06.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `least_model` and `distribution_semantics` exactly as
   in the Week 6 lecture content.
2. **Task B — Worked example:** reproduce the `rains`/`sprinkler_on`/`wet` example; confirm
   `distribution_semantics` returns 0.72, matching the by-hand total-choice enumeration.
3. **Task C — Three-fact extension:** add `broken_sprinkler` and a `dry_lawn` query as in the
   Week 6 exercise; compute `P(dry_lawn)` by hand via total-choice enumeration and verify against
   your code.
4. **Task D — MLN contrast:** write a short markdown cell giving the weighted first-order
   formulas an MLN encoding of the same `wet` scenario would use (reusing the graduate course's
   MLN evaluator conceptually, not re-implementing it), and state in 2–3 sentences where a
   partition function Z would enter that this week's semantics does not need.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required written contrast.

## Submission
Export/submit `lab06.ipynb` via the course submission system by the end of the lab session.
