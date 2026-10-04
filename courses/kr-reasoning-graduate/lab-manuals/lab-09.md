# Lab Manual 9 — Brute-Force MLN Evaluator

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a brute-force Markov Logic Network evaluator over a small domain and observe how
weight changes shift relative world probabilities.

## Setup
1. Reuse your course virtual environment.
2. Create `lab09.ipynb`.

## Procedure
1. **Task A — MLN evaluator:** implement `ground_atoms`, `count_satisfied_groundings`, and
   `mln_distribution` from the Week 9 lecture content. Build the single-formula toy MLN from §6
   (just F2 = ∀x.¬Smokes(x)) over 2 constants and compute the full distribution.
2. **Task B — Hard-constraint limit:** raise F2's weight to a large value (e.g., 20) and confirm
   the distribution concentrates almost entirely on the all-non-smoking world, as described in
   §6.
3. **Task C — Two-formula MLN:** add F1 (the Friends/Smokes correlation formula from §4) with a
   moderate weight and compute the full distribution over the 6-ground-atom, 2-constant domain;
   identify the highest-probability world and explain, in a markdown cell, why it is the most
   probable given both formulas' weights.
4. **Task D — Mini-challenge:** compute, by hand, n_i(x) (the count of satisfied groundings) for
   F1 on one specific world of your choosing, and confirm your hand count matches
   `count_satisfied_groundings`'s output.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab09.ipynb` via the course submission system by the end of the lab session.
