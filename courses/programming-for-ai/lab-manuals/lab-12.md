# Lab Manual 12 — Clustering & PCA

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Apply k-means clustering and PCA for dimensionality reduction/visualization.

## Setup
Create `lab12.ipynb`; use a provided unlabeled (or label-stripped) dataset with ≥4 numeric
features.

## Procedure
1. **Task A — Elbow method:** run k-means for k = 1..10; plot inertia vs. k; choose and justify
   a value of k.
2. **Task B — Clustering:** fit k-means with the chosen k; report cluster sizes.
3. **Task C — PCA visualization:** reduce the data to 2D with PCA; scatter-plot points colored
   by cluster label; report `explained_variance_ratio_`.
4. **Task D — Reflection:** if the dataset originally had labels, compare clusters to the true
   labels (qualitatively); if not, describe what the clusters seem to represent.

## Expected Output
A notebook with Tasks A–D.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
