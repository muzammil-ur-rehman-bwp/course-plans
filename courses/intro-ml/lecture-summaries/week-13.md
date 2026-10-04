# Week 13 Summary — Dimensionality Reduction & Anomaly Detection

**Key takeaways:**
- PCA finds orthogonal directions (principal components) that successively capture the most
  variance in scaled data; explained variance ratio guides how many components to keep.
- PCA is useful both for 2D visualization of high-dimensional data and as a preprocessing step
  before modeling.
- Simple statistical thresholds (z-score, IQR) detect single-feature outliers; `IsolationForest`
  and `LocalOutlierFactor` detect multivariate anomalies that single-feature rules would miss.

**You should now be able to:** fit PCA, choose the number of components via explained variance,
and apply model-based anomaly detection to flag unusual examples.

**Next week:** recommender systems — content-based and collaborative filtering.
