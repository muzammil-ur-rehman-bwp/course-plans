# Lab Manual 5 — Independent Q-Learners in Repeated Matrix Games

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement independent Q-learners for both players of a repeated matrix game and empirically
observe how non-stationarity produces cyclic, non-convergent behavior in a zero-sum game with no
pure-strategy Nash equilibrium, in contrast to stable convergence in a coordination game.

## Setup
1. Reuse your course virtual environment.
2. Create `lab05.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `independent_q_learning_step` and
   `run_independent_learners` exactly as in the Week 5 lecture content.
2. **Task B — Matching pennies:** define the payoff matrices for a repeated matching-pennies-
   style zero-sum game (no pure-strategy Nash equilibrium). Run `run_independent_learners` for
   20,000 episodes and plot each player's action-frequency trajectory (e.g., a rolling window of
   recent actions) over the run.
3. **Task C — Coordination game:** define payoff matrices for a repeated coordination game where
   a pure-strategy Nash equilibrium exists and is also the social optimum. Run the same learners
   for 20,000 episodes and plot the same action-frequency trajectories.
4. **Task D — Mini-challenge:** implement a minimal joint-action learner that maintains a table
   Q(a, b) over joint actions (rather than separate Qa and Qb) for the matching-pennies game from
   Task B, and compare its action-frequency trajectory to the independent learners' from Task B.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the two action-frequency plots (Tasks B and C) plus the Task D comparison.

## Submission
Export/submit `lab05.ipynb` via the course submission system by the end of the lab session.
