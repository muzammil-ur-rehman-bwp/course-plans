# Week 12 Lecture Plan — Introduction to Machine Learning
## Topic: Unsupervised Learning I — Clustering

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the k-means and hierarchical clustering algorithms. (*Understand*)
2. Apply `KMeans` and `AgglomerativeClustering`, choosing `k` via the elbow method. (*Apply*)
3. Analyze clustering quality using the silhouette score. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | k-means algorithm | Initialization, assign step, update step, convergence |
| 0:25–0:45 | Choosing k | The elbow method (inertia vs. k) |
| 0:45–0:55 | Break | — |
| 0:55–1:20 | Hierarchical clustering | Agglomerative merging; linkage criteria; dendrograms |
| 1:20–1:45 | Silhouette score | Formula and interpretation; comparing clustering configurations |
| 1:45–2:00 | When to use which | Shape assumptions, scalability, interpretability tradeoffs |

### Materials/Equipment
- Live-coding environment, scikit-learn, SciPy; a 2D dataset with visually separable clusters.

### Formative Check (in-class)
Exercise: run k-means at `k=2,3,4,5` on a toy dataset and pick the best `k` using both the elbow
plot and the silhouette score.

### Link to Lab/Assessment
Lab 12: clustering lab with k-means, hierarchical clustering, a dendrogram, and silhouette-score
comparison.
