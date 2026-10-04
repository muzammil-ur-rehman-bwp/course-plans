# Lab Manual 15 — Pipelines, Persistence & Fairness Audit

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Build and persist a full ML pipeline, and audit it for a fairness gap across a sensitive
attribute.

## Setup
Continue with scikit-learn/joblib; create `lab15.ipynb`.

## Procedure
1. **Task A — Full pipeline:** build a `Pipeline` combining a `ColumnTransformer` (reuse/adapt
   Week 11's) with a classifier of your choice; fit it inside a small `GridSearchCV`.
2. **Task B — Persist:** save the best pipeline with `joblib.dump`; in a new cell (simulating a
   fresh process), reload it with `joblib.load` and confirm it reproduces the same predictions on
   the test set.
3. **Task C — Input validation:** write a small function that checks a new input DataFrame has
   the expected columns before calling `.predict()`, and raises a clear error otherwise; test it
   with both valid and deliberately malformed input.
4. **Task D — Fairness audit:** using the provided sensitive-attribute column, compute accuracy
   and recall separately for each group; report the gap and write 2–3 sentences discussing
   whether it would be acceptable to deploy as-is.

## Expected Output
A notebook with Tasks A–D, including the group-wise metrics table for Task D.

## Submission
Submit `lab15.ipynb` by the end of the lab session.
