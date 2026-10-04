# Lab Manual 12 — PCA and Kernel PCA From Scratch

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement PCA via eigendecomposition and kernel PCA from scratch, cross-checked against
scikit-learn, and compare both to nonlinear manifold learning on a curved dataset.

## Setup
Create `lab12.ipynb`. NumPy, SciPy, scikit-learn.

## Procedure
1. **Task A — PCA from scratch:** implement PCA via eigendecomposition of the covariance matrix,
   as in the lecture content; cross-check projections and explained-variance ratios against
   `sklearn.decomposition.PCA`.
2. **Task B — Kernel PCA from scratch:** implement the centered-kernel-matrix kernel PCA
   procedure for an RBF kernel; apply it to the concentric-circles dataset from the lecture and
   confirm it separates the two circles along one component.
3. **Task C — Swiss roll:** generate a Swiss-roll dataset (`sklearn.datasets.make_swiss_roll`);
   apply linear PCA, kernel PCA, and `sklearn.manifold.Isomap`; visualize all three 2-D
   embeddings side by side.
4. **Task D — t-SNE:** apply `sklearn.manifold.TSNE` to the same Swiss-roll data; compare its
   embedding to Isomap's.
5. **Task E — Reflection:** explain, in 3–4 sentences, which method(s) preserved the manifold's
   intrinsic structure best and why, referencing the lecture's geodesic-distance vs.
   local-probability-matching distinction.

## Expected Output
A notebook with Tasks A–E and the Task C/D comparison figure.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
