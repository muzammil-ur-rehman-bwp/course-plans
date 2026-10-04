# Week 12 Summary — Unsupervised Learning

**Key takeaways:**
- k-means iteratively assigns points to the nearest centroid and recomputes centroids until
  convergence; it assumes roughly spherical, similarly sized clusters.
- The elbow method (plotting inertia vs. k) helps choose a reasonable number of clusters.
- PCA projects high-dimensional data onto the directions of greatest variance, enabling 2D/3D
  visualization and `explained_variance_ratio_` tells you how much information is retained.
- Combining k-means + PCA (cluster, then visualize via PCA) is a standard unsupervised-learning
  inspection pattern.

**You should now be able to:** run and tune k-means; choose k via the elbow method; apply PCA
for dimensionality reduction and visualization.

**Next week:** model evaluation in depth — overfitting, cross-validation, hyperparameter tuning.
