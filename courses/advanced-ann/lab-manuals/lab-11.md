# Lab Manual 11 — Computing a PAC-Bayes Bound

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Compute a simple PAC-Bayes-style bound for a small trained network under a Gaussian-perturbation
posterior, and compare its numerical value to a classical VC-style bound's value on the same
network.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab11.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `empirical_risk_under_posterior` and the PAC-Bayes bound
   computation exactly as in the Week 11 lecture content, and reproduce its reported comparison.
2. **Task B — Posterior-variance sweep:** compute the PAC-Bayes bound for at least 5 values of
   `sigma_q2` spanning at least two orders of magnitude, plotting both the empirical-risk term and
   the full bound against `sigma_q2`, and identify the value that minimizes the bound.
3. **Task C — Sharpness connection:** train a second network of the same architecture using SAM
   (reuse `sam_step` from Week 6's lab) on the same data, and compare its minimized PAC-Bayes
   bound (from a Task B-style sweep) to the plain-SGD-trained network's, connecting the result to
   Week 11 §3's sharpness argument.
4. **Task D — Mini-challenge:** vary the training-set size `n` across at least 3 values, holding
   everything else fixed, and report how the minimized PAC-Bayes bound changes, checking it
   behaves consistently with the bound's explicit $1/\sqrt n$-type dependence.

## Expected Output
A notebook with four clearly labeled sections (A–D), including all required plots and comparisons.

## Submission
Export/submit `lab11.ipynb` via the course submission system by the end of the lab session.
