# Lab Manual 6 — Equilibrium Computation: Nash vs. Correlated

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Compute Nash equilibria for a zero-sum game (via fictitious play) and a general-sum game (via
support enumeration), compute a correlated equilibrium of the general-sum game via linear
programming, and compare the social welfare achievable under each solution concept.

## Setup
1. Reuse your course virtual environment.
2. Create `lab06.ipynb`. `scipy` is optional (used only if available; a from-scratch fallback is
   provided for correlated-equilibrium computation if it is not).

## Procedure
1. **Task A — Zero-sum game:** for a provided 3x3 zero-sum payoff matrix, run
   `solve_zero_sum_fictitious_play` exactly as in the Week 6 lecture content. Report the
   estimated game value, and, if `scipy.optimize.linprog` is available, verify it against the
   exact LP solution.
2. **Task B — Support enumeration:** for a provided 2x2 general-sum bimatrix game with a known
   mixed-strategy equilibrium, complete a from-scratch support-enumeration function (finishing
   the sketch in the Week 6 lecture content) and confirm it recovers the known equilibrium.
3. **Task C — Correlated equilibrium:** for the same general-sum game as Task B, set up and
   solve the correlated-equilibrium linear program (via `scipy.optimize.linprog` if available, or
   a from-scratch iterative fallback otherwise). Report the joint distribution found and its
   expected social welfare.
4. **Task D — Mini-challenge:** compare the best social welfare achievable under the correlated
   equilibrium (Task C) to the social welfare of the Nash equilibrium found in Task B. In 2–3
   sentences, explain why a correlated equilibrium can achieve strictly higher social welfare.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the welfare comparison described in Task D.

## Submission
Export/submit `lab06.ipynb` via the course submission system by the end of the lab session.
