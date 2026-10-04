# Lab Manual 2 — Multiplicative Weights

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture

## Objectives
Implement the multiplicative-weights algorithm from scratch and empirically verify its regret
bound against the best fixed expert in hindsight.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab02.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `multiplicative_weights(loss_matrix, eta)` exactly as
   in the Week 2 lecture content.
2. **Task B — Regret measurement:** generate a loss matrix for N=5 experts over T=2000 rounds
   (one expert should be secretly better on average but with substantial per-round noise). Run
   your implementation with η = √(ln 5 / 2000) and plot cumulative learner loss, the best fixed
   expert's cumulative loss, and the running regret (their difference) over time.
3. **Task C — Bound verification:** compute the theoretical bound ηT + (ln N)/η for your chosen η
   and T, and confirm your empirical regret curve stays below it at every round.
4. **Task D — Mini-challenge:** sweep η over at least 5 values (including one much too large and
   one much too small) and plot final regret vs. η, confirming the U-shaped curve the
   ηT + (ln N)/η formula predicts, with a minimum near the theoretically optimal η.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the three required plots.

## Submission
Export/submit `lab02.ipynb` via the course submission system by the end of the lab session.
