# Lab Manual 7 — Adam From Scratch, Non-Convergence, and Warmup

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement Adam from scratch, reproduce a small non-convergence example, and implement a
learning-rate warmup+decay schedule, comparing early-training stability with and without warmup.

## Setup
Create `lab07.ipynb`. Start from the lecture's `run_adam` and `run_with_schedule` functions.

## Procedure
1. **Task A — Adam from scratch:** implement Adam's update rule (with bias correction) as a
   reusable function/class (not only the toy scalar version); apply it to train a small MLP on a
   synthetic regression task and confirm the loss decreases.
2. **Task B — Non-convergence reproduction:** reproduce the lecture's oscillating-gradient
   non-convergence sketch for plain Adam, and implement AMSGrad's running-maximum fix; plot
   $\theta_t$ over time for both and confirm AMSGrad stabilizes while plain Adam drifts.
3. **Task C — Warmup comparison:** reproduce the lecture's ill-conditioned-quadratic warmup
   comparison; plot loss vs. step for `warmup_steps=0` and `warmup_steps=50` on the same chart
   (log-y axis) and report at what step the no-warmup run's loss first exceeds $10\times$ its
   initial value.
4. **Task D — Your own schedule:** design and test one additional learning-rate schedule (e.g.,
   cosine decay after warmup) on the Task A regression task, and report whether it trains faster
   or more stably than a constant learning rate at the same peak value.

## Expected Output
A notebook with Tasks A–D, including both required plots.

## Submission
Submit `lab07.ipynb` by the end of the lab session.
