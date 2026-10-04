# Lab Manual 11 — Approximate Inference: Rejection Sampling & Likelihood Weighting

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Implement rejection sampling and likelihood weighting on a small Bayesian network and compare
their efficiency.

## Setup
1. Reuse your course virtual environment.
2. Create `lab11.ipynb`.

## Procedure
1. **Task A — Bayesian network encoding:** encode a 4-node Bayesian network (e.g., a classic
   Burglary/Earthquake/Alarm/JohnCalls-style network) with CPTs and a topological ordering,
   using the `cpt`/`parents` structure from the Week 11 lecture content.
2. **Task B — Rejection sampling:** implement `rejection_sampling`; estimate P(Burglary | JohnCalls
   = True) with 10,000 samples and report the fraction of samples kept.
3. **Task C — Likelihood weighting:** implement `likelihood_weighting`; estimate the same query
   with the same sample budget and compare the result and the fraction of "useful" (high-weight)
   samples to Task B.
4. **Task D — Mini-challenge:** repeat both methods 10 times each (different random seeds) at a
   rarer evidence setting (e.g., condition on a low-probability combination of evidence
   variables) and report the mean and standard deviation of each method's estimate, showing
   which method has lower variance.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output.

## Submission
Export/submit `lab11.ipynb` via the course submission system by the end of the lab session.
