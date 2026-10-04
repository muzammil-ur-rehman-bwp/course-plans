# Lab Manual 3 — UCB1 vs. ε-Greedy

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement UCB1 and empirically compare its cumulative regret against ε-greedy on simulated
Bernoulli bandits, verifying UCB1's logarithmic regret growth.

## Setup
1. Reuse your course virtual environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `ucb1(bandit_means, T, rng)` exactly as in the Week 3
   lecture content, and a matching ε-greedy implementation.
2. **Task B — Simulation:** simulate K=5 Bernoulli arms with means [0.1, 0.3, 0.5, 0.55, 0.6].
   Run both algorithms for T=5000 rounds, repeated over 50 random seeds.
3. **Task C — Comparison:** plot mean cumulative regret with a spread band (e.g., ±1 standard
   deviation across seeds) for both algorithms on the same axes; fit a log(T) curve to UCB1's
   regret and a linear curve to ε-greedy's long-run regret, and report both fits' quality.
4. **Task D — Mini-challenge:** implement Thompson sampling with a Beta-Bernoulli posterior for
   the same bandit, and add its regret curve to the Task C plot.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the comparison plot described in Task C.

## Submission
Export/submit `lab03.ipynb` via the course submission system by the end of the lab session.
