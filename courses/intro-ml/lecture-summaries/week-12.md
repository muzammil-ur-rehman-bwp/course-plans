# Week 12 Summary — Unsupervised Learning I — Clustering

**Key takeaways:**
- k-means alternates assigning points to the nearest centroid and recomputing centroids,
  minimizing within-cluster squared distance; it requires `k` in advance and scaled features.
- The elbow method (inertia vs. `k`) and the silhouette score both help choose `k`; silhouette
  can compare across different `k` more reliably since inertia always decreases with more
  clusters.
- Agglomerative clustering merges clusters bottom-up according to a linkage criterion, producing
  a dendrogram that can be cut at any number of clusters.
- Different clustering shape assumptions and scalability profiles make k-means and hierarchical
  clustering suited to different situations.

**You should now be able to:** fit k-means and agglomerative clustering; choose `k` using the
elbow method and silhouette score; read and cut a dendrogram.

**Next week:** unsupervised learning II — PCA and dimensionality reduction, and anomaly
detection.
