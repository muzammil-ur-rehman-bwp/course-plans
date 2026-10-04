# Lab Manual 5 — FTRL and Online Gradient Descent

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement online gradient descent and FTRL from scratch and empirically compare their regret on
a synthetic sequence of convex losses, verifying the derived $O(\sqrt T)$ regret bound.

## Setup
1. Reuse your virtual environment.
2. Create `lab05.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `online_gradient_descent` and the closed-form
   quadratic-loss `ftrl_quadratic_reg` exactly as in the Week 5 lecture content.
2. **Task B — Regret comparison:** run both algorithms on the same $T=500$, $d=2$ quadratic loss
   sequence, plot both regret curves against the best fixed point in hindsight on the same axes.
3. **Task C — Bound verification:** compute the theoretical bound $DG\sqrt T$ for your chosen
   $D,G$ and confirm OGD's empirical regret curve stays below it at every round.
4. **Task D — Mini-challenge:** construct a loss sequence built from the absolute-value function
   (e.g., $f_t(x)=|x-a_t|$ in 1-D) and compare OGD and FTRL's regret curves on this new sequence;
   discuss, in 2–3 sentences, why FTRL's closed-form running-average update (derived for quadratic
   losses) no longer applies directly here and what would need to change.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required regret plots.

## Submission
Export/submit `lab05.ipynb` via the course submission system by the end of the lab session.
