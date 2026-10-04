# Lab Manual 4 — Max-Margin Implicit Bias of Gradient Descent

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture

## Objectives
Run gradient descent on separable synthetic data and empirically confirm convergence, in
direction, to the max-margin classifier predicted by Week 4's derivation.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab04.ipynb`.

## Procedure
1. **Task A — Implementation:** implement the gradient-descent training loop and the penalized
   hard-margin solver exactly as in the Week 4 lecture content.
2. **Task B — Convergence tracking:** run gradient descent for at least 20,000 steps, recording
   cosine similarity to the max-margin direction at regular intervals, and plot the trajectory.
3. **Task C — Norm growth:** additionally record $\|w_t\|$ at the same checkpoints and plot it
   against $\log(t)$; report whether the relationship looks approximately linear, as Week 4 §3
   Step 4 predicts.
4. **Task D — Tail sensitivity (mini-challenge):** repeat Task B using the exponential loss
   $\ell(z) = e^{-z}$ in place of the logistic loss, and confirm convergence to the same max-margin
   direction, consistent with Week 4 §4's claim that the tail behavior, not the exact loss form,
   drives the result.

## Expected Output
A notebook with four clearly labeled sections (A–D), including all required plots.

## Submission
Export/submit `lab04.ipynb` via the course submission system by the end of the lab session.
