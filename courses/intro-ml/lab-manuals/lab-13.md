# Lab Manual 13 — PCA & Anomaly Detection

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Apply PCA for dimensionality reduction and visualization, and detect anomalies with
`IsolationForest`.

## Setup
Continue with scikit-learn; create `lab13.ipynb`.

## Procedure
1. **Task A — Explained variance:** on the provided (scaled) high-dimensional dataset, fit full
   `PCA`; plot cumulative explained variance and report the number of components needed for 90%.
2. **Task B — 2D visualization:** project the data onto its first two components; scatter-plot
   colored by class/label and describe what structure, if any, is visible.
3. **Task C — PCA as preprocessing:** fit a classifier (e.g., k-NN) on the raw scaled features vs.
   on the top-k PCA components from Task A; compare test accuracy and training time.
4. **Task D — Anomaly detection:** fit `IsolationForest` on the provided dataset with injected
   outliers; report how many points are flagged, and spot-check a few flagged points.

## Expected Output
A notebook with Tasks A–D, including the scree plot and the 2D PCA scatter plot.

## Submission
Submit `lab13.ipynb` by the end of the lab session.
