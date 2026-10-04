# Lab Manual 12 — Clustering

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Apply k-means and agglomerative clustering, choose the number of clusters, and evaluate with the
silhouette score.

## Setup
Continue with scikit-learn/SciPy; create `lab12.ipynb`.

## Procedure
1. **Task A — k-means + elbow:** on the provided (scaled) dataset, fit `KMeans` for `k` in
   `1..9`; plot the elbow curve and identify a candidate `k`.
2. **Task B — Silhouette:** compute the silhouette score for `k` in `2..9`; identify the `k` that
   maximizes it, and compare to Task A's elbow-based choice.
3. **Task C — Agglomerative clustering:** fit `AgglomerativeClustering` with your chosen `k` and
   `linkage="ward"`; plot a dendrogram and mark where it would be cut for that `k`.
4. **Task D — Compare:** compare k-means and agglomerative cluster assignments at the same `k`
   (e.g., via a cross-tabulation); report whether the two methods largely agree.

## Expected Output
A notebook with Tasks A–D, including the elbow plot, silhouette plot, and dendrogram.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
