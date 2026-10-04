# Lab Manual 11 — REINFORCE with a Baseline

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Implement REINFORCE with a return baseline on a toy environment and compare gradient-estimate
variance with and without the baseline.

## Setup
Create `lab11.ipynb`. Start from the lecture's `PolicyNetwork`, `compute_returns`, and
`reinforce_update`. Use `gymnasium`'s `CartPole-v1` if available, otherwise a custom
grid-world/bandit.

## Procedure
1. **Task A — REINFORCE with baseline:** implement the full training loop with the batch-mean
   baseline; train until the agent reaches a reasonable return threshold; plot episode return.
2. **Task B — REINFORCE without baseline:** retrain with `baseline=False`; plot episode return
   and compare learning-curve smoothness/variance against Task A.
3. **Task C — Gradient variance measurement:** for a fixed policy snapshot, sample the
   policy-gradient estimator many times (many trajectory rollouts) with and without the baseline,
   and report the empirical variance of the resulting gradient estimates for one parameter.
4. **Task D — Discussion:** in a markdown cell, relate Task C's measured variance reduction to
   the unbiasedness argument from lecture.

## Expected Output
A notebook with Tasks A–D; Task C's variance comparison reported numerically.

## Submission
Submit `lab11.ipynb` by the end of the lab session.
