# Lab Manual 10 — Permutation Importance and a Sanity-Check Failure

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Compute permutation importance on a synthetic dataset with a known ground-truth feature
relevance, verify it correctly identifies the relevant features, and demonstrate — using the
randomization sanity check — why a naive raw-correlation attribution method fails by construction.

## Setup
1. Reuse your course virtual environment.
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Dataset and model:** build a synthetic classification dataset with 6 features
   where the true label depends only on features 0 and 1 (via a simple known rule), with
   features 2–5 pure noise. Fit a simple classifier (e.g., logistic regression).
2. **Task B — Permutation importance:** implement `permutation_importance` exactly as in the
   Week 10 lecture content. Compute it for the fitted model and confirm features 0 and 1 rank
   above features 2–5.
3. **Task C — Sanity check:** implement a naive attribution method that simply returns each
   feature's raw correlation with the model's output, ignoring the model's actual parameters.
   Implement `randomization_sanity_check` exactly as in the lecture content, run it on this naive
   method, and discuss, in 2–3 sentences, why it fails the check by construction.
4. **Task D — Mini-challenge:** run `randomization_sanity_check` on `permutation_importance`
   itself (as the `attribution_fn`) and report whether it passes. In 2–3 sentences, discuss what
   passing or failing this check would mean for trusting permutation importance as evidence of
   genuine mechanism, vs. mere correlation.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output, including the Task B importance ranking and the Task C/D sanity-check results.

## Submission
Export/submit `lab10.ipynb` via the course submission system by the end of the lab session.
