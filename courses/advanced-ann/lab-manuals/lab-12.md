# Lab Manual 12 — Mode Connectivity: Linear vs. Nonlinear Interpolation

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement linear interpolation and a bend-point nonlinear (Bezier) path between two independently
trained minima, and compare loss along each, demonstrating mode connectivity directly.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab12.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `train`, `flatten`, `set_params`, `loss_at`, the linear
   interpolation loop, and the Bezier bend-point optimization exactly as in the Week 12 lecture
   content.
2. **Task B — Barrier measurement:** evaluate loss at a finer grid of at least 11 $\lambda$ values
   along the linear path, plot it, and report the barrier height (peak loss minus the loss at
   $\lambda=0$ and $\lambda=1$).
3. **Task C — Nonlinear path:** optimize the bend point and evaluate loss along the Bezier path at
   the same grid of $\lambda$ values, plotting both curves on the same axes for direct comparison.
4. **Task D — Winning-ticket connectivity (mini-challenge):** train two networks from the *same*
   initialization but different random data orders (simulating a winning-ticket-style shared
   initialization), and repeat Task B's linear-interpolation barrier measurement between them;
   compare the barrier height to the Task B result between independently initialized networks,
   connecting to Week 12 §3's linear-mode-connectivity refinement.

## Expected Output
A notebook with four clearly labeled sections (A–D), including all required plots and the Task D
comparison.

## Submission
Export/submit `lab12.ipynb` via the course submission system by the end of the lab session.
