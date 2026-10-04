# Lab Manual 8 — Value Iteration on a Grid-World MDP

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement value iteration on a small grid-world MDP and examine convergence behavior under
different discount factors.

## Setup
1. Reuse your course virtual environment.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — Grid-world MDP:** encode a 4x4 grid-world with a +1 goal, a −1 trap, −0.04 step
   cost elsewhere, and a small chance (e.g., 0.1) of "slipping" to a perpendicular cell instead
   of the intended direction (a stochastic transition model).
2. **Task B — Value iteration:** implement `value_iteration` from the Week 8 lecture content;
   run with γ = 0.9 and report the converged value function and the number of iterations to
   convergence (θ = 1e-6).
3. **Task C — Policy extraction:** implement `extract_policy` from the Week 8 lecture content
   and display the resulting optimal policy as a grid of arrows/action labels.
4. **Task D — Mini-challenge:** re-run Task B with γ = 0.5 and γ = 0.99; report how many
   iterations each takes to converge and discuss, referencing the Week 8 contraction argument,
   why a smaller γ converges faster.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab08.ipynb` via the course submission system by the end of the lab session.
