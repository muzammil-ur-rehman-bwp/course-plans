# Lab Manual 6 — Gradient Descent Convergence Rates

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Empirically verify the $O(1/T)$ convex and linear strongly-convex gradient-descent convergence
rates derived in lecture.

## Setup
Create `lab06.ipynb`. NumPy, Matplotlib.

## Procedure
1. **Task A — Implement GD:** implement batch gradient descent for a general quadratic objective
   $f(x)=\frac12 x^\top Ax$, as in the lecture content.
2. **Task B — Convex case:** construct a PSD but singular $A$ (merely convex, not strongly
   convex); run GD with $\eta=1/L$; plot $f(x_t)-f^\star$ vs. $t$ on a log-log scale and confirm
   the slope is consistent with $O(1/T)$.
3. **Task C — Strongly convex case:** add $\mu I$ to $A$; repeat Task B on a log-linear scale and
   confirm a straight-line (geometric) decay.
4. **Task D — Condition number sweep:** vary $\mu$ (holding $L$ fixed) and measure how many
   iterations are needed to reach $f(x_T)-f^\star<10^{-6}$; relate this to the condition number
   $L/\mu$.
5. **Task E — KKT/SVM derivation:** by hand (written up in a markdown cell), derive the SVM dual
   from the primal via the KKT conditions, as in the lecture content, and verify
   $w^\star=\sum_i\alpha_iy_ix_i$ numerically on a small toy linearly-separable dataset solved with
   `scipy.optimize.minimize` on the dual.

## Expected Output
A notebook with Tasks A–E, including both convergence plots and the condition-number table.

## Submission
Submit `lab06.ipynb` by the end of the lab session.
