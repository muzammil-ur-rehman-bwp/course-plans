# Lab Manual 9 — Support Vector Machines

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Fit SVMs with linear, polynomial, and RBF kernels, and study the effect of `C` and `gamma`.

## Setup
Continue with scikit-learn; create `lab09.ipynb`.

## Procedure
1. **Task A — Linear kernel:** fit `SVC(kernel="linear")` (inside a scaled pipeline) on the
   provided linearly-separable-ish dataset; report test accuracy and plot the decision boundary.
2. **Task B — Non-linear data:** on the provided non-linearly-separable dataset, fit linear,
   polynomial (`degree=3`), and RBF kernels; report test accuracy for each and plot all three
   decision boundaries side by side.
3. **Task C — Tuning C:** on the RBF kernel, sweep `C` in `{0.01, 0.1, 1, 10, 100}` (fixed
   `gamma="scale"`); report test accuracy for each and identify the best.
4. **Task D — Tuning gamma:** at your best `C` from Task C, sweep `gamma` in
   `{0.01, 0.1, 1, 10}`; report test accuracy and describe how the decision boundary changes
   qualitatively as `gamma` increases.

## Expected Output
A notebook with Tasks A–D, including all decision-boundary plots.

## Submission
Submit `lab09.ipynb` by the end of the lab session.
