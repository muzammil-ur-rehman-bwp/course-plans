# Lab Manual 10 — Distance-Based Belief-Merging Evaluator

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Implement a distance-based belief-merging evaluator and check it against the merging postulates
on a worked multi-agent example.

## Setup
1. Reuse your Week 1 environment, plus your graduate course's Dalal-revision model-enumeration
   code (reused, not re-derived).
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `hamming`, `all_valuations`, `models_of`,
   `distance_to_set`, and `merge` exactly as in the Week 10 lecture content.
2. **Task B — Worked example:** encode the Week 10 three-agent, three-variable `(p,q,r)` example
   under integrity constraint `r`; compute the merged result's models under sum aggregation.
3. **Task C — Aggregation comparison:** recompute under max aggregation; report whether the two
   aggregations agree, and if not, identify the differing valuation and explain in one sentence
   what it says about fairness to the most-violated agent.
4. **Task D — Postulate check:** verify IC0 (result entails IC) and IC2 (commutativity — reorder
   the input profiles and confirm the result is unchanged) on your Task B/C example.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required written comparisons.

## Submission
Export/submit `lab10.ipynb` via the course submission system by the end of the lab session.
