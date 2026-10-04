# Lab Notes 13 — PCA & Anomaly Detection

**Concept recap:** PCA finds variance-maximizing orthogonal directions in scaled data; anomaly
detectors flag points that are statistically or structurally unusual.

**Common pitfalls:**
- Running PCA on unscaled features — a feature with a much larger numeric range will dominate
  the variance calculation and the resulting components, regardless of its actual importance.
- Over-interpreting individual principal components as meaningful real-world concepts — they are
  linear combinations of original features and are not guaranteed to correspond to anything
  intuitive.
- Setting `IsolationForest`'s `contamination` parameter to a value that doesn't match the actual
  (even if unknown) outlier rate, which can flag far too many or too few points as anomalies.

**Debugging tip:** if PCA's first component "explains" almost all the variance in a way that
seems too good, check whether one feature is on a wildly different scale and dominating the
(supposedly scaled) data — verify the scaler was actually applied before PCA.

**Instructor tip:** have students inspect the actual feature loadings (`pca.components_`) for
the first component on a dataset with interpretable features, to connect the abstract "direction
of maximum variance" to real feature contributions.
