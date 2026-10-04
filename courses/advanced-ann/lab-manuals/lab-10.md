# Lab Manual 10 — Reproducing Model-Size Double Descent

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Reproduce a model-size double-descent curve on a controlled random-feature-regression dataset and
identify the interpolation threshold.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab10.ipynb`.

## Procedure
1. **Task A — Implementation:** implement `make_data` and `train_random_feature_model` exactly as
   in the Week 10 lecture content.
2. **Task B — Width sweep:** run the width sweep exactly as in the lecture content (at least the
   10 listed widths), and plot test MSE against width.
3. **Task C — Threshold identification:** identify the width at which test MSE peaks, and confirm
   it is close to `n_train`, as the interpolation-threshold argument predicts.
4. **Task D — Sample-size axis (mini-challenge):** holding width fixed at a value near (but not
   exactly at) the Task C peak, sweep `n_train` across a range that crosses the resulting
   width-specific interpolation threshold, and plot test MSE against `n_train`, reporting whether
   a peak appears at the predicted location.

## Expected Output
A notebook with four clearly labeled sections (A–D), including both required plots.

## Submission
Export/submit `lab10.ipynb` via the course submission system by the end of the lab session.
