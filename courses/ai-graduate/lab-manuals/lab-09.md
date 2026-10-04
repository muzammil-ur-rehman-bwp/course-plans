# Lab Manual 9 — Policy Iteration & Q-Learning

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement policy iteration and tabular Q-learning on the grid-world MDP, and compare
model-based to model-free learning.

## Setup
1. Reuse your course virtual environment and Week 8's grid-world encoding.
2. Create `lab09.ipynb`.

## Procedure
1. **Task A — Policy iteration:** implement `policy_evaluation`/`policy_iteration` from the
   Week 9 lecture content; confirm the resulting optimal policy matches Week 8's value-iteration
   policy exactly.
2. **Task B — Q-learning:** implement `q_learning` from the Week 9 lecture content with
   ε = 0.1, α = 0.1, γ = 0.9, run for several thousand episodes; report the learned greedy
   policy and compare it to Task A's.
3. **Task C — Exploration comparison:** re-run Q-learning with ε = 0 (pure exploitation from an
   all-zero initial Q-table) and report what goes wrong.
4. **Task D — Mini-challenge:** plot cumulative reward per episode over training for ε = 0.1,
   ε = 0.3, and ε = 0, and discuss the exploration-exploitation tradeoff visible in the plot.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the Task D plot.

## Submission
Export/submit `lab09.ipynb` via the course submission system by the end of the lab session.
