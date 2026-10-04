# Week 13 — Lecture Content: Dimensionality Reduction & Anomaly Detection

## 1. Principal Component Analysis (PCA): Intuition
PCA finds a new set of axes (**principal components**), ordered so that the first captures the
most variance in the data, the second captures the most remaining variance subject to being
orthogonal to the first, and so on. Projecting onto the first few components reduces
dimensionality while preserving as much of the data's variance as possible.

Formally, PCA finds unit vectors `v_1, v_2, ...` (the components) such that the projection
`X v_1` has maximum variance, `X v_2` has maximum variance subject to `v_2 ⊥ v_1`, and so on.
These turn out to be the eigenvectors of the (centered) data's covariance matrix, ordered by
eigenvalue (the variance each component explains).

## 2. PCA in scikit-learn
```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline

# PCA is distance/variance-based: features must be scaled first, exactly as for k-means/SVM/k-NN.
pca = make_pipeline(StandardScaler(), PCA(n_components=5))
X_reduced = pca.fit_transform(X_train)

print("Explained variance ratio:", pca.named_steps["pca"].explained_variance_ratio_)
print("Cumulative:", pca.named_steps["pca"].explained_variance_ratio_.cumsum())
```
The **explained variance ratio** for each component is the fraction of total variance it
captures; the cumulative sum tells you how many components are needed to retain, e.g., 90% of
the original variance — a common rule of thumb for choosing `n_components`.

```python
import matplotlib.pyplot as plt

pca_full = PCA().fit(StandardScaler().fit_transform(X_train))
plt.plot(range(1, len(pca_full.explained_variance_ratio_) + 1),
         pca_full.explained_variance_ratio_.cumsum(), marker="o")
plt.xlabel("Number of components"); plt.ylabel("Cumulative explained variance")
plt.axhline(0.9, color="red", linestyle="--")
plt.show()
```

## 3. PCA for Visualization
Projecting high-dimensional data onto its first two principal components allows visualizing
structure (e.g., class separation, cluster structure) that can't be plotted directly:
```python
pca_2d = make_pipeline(StandardScaler(), PCA(n_components=2))
X_2d = pca_2d.fit_transform(X_train)

plt.scatter(X_2d[:, 0], X_2d[:, 1], c=y_train, alpha=0.7)
plt.xlabel("PC1"); plt.ylabel("PC2")
plt.title("2D PCA projection")
plt.show()
```
PCA is also commonly used **as a preprocessing step** before a model (e.g., k-NN or clustering)
on high-dimensional data, both to mitigate the curse of dimensionality (Week 5) and to speed up
training — at the cost of some interpretability, since each component is a linear combination of
original features.

## 4. Anomaly/Outlier Detection: Statistical Thresholds
The simplest anomaly detectors use a statistical threshold on a single feature:
- **Z-score:** flag `x` as an outlier if `|x - mean| / std > threshold` (e.g., 3).
- **IQR rule:** flag `x` as an outlier if it falls below `Q1 - 1.5*IQR` or above
  `Q3 + 1.5*IQR`, where `IQR = Q3 - Q1`.
These are simple and interpretable but only consider one feature at a time, missing anomalies
that are only unusual in combination across several features.

## 5. Model-Based Anomaly Detection
- **`IsolationForest`:** builds an ensemble of random trees that isolate each point by
  recursively partitioning the feature space; anomalies, being "few and different," tend to be
  isolated in fewer splits (shorter average path length) than normal points.
```python
from sklearn.ensemble import IsolationForest

iso = IsolationForest(contamination=0.05, random_state=42)
iso.fit(X_train_scaled)
anomaly_labels = iso.predict(X_test_scaled)   # -1 = anomaly, 1 = normal
```
- **`LocalOutlierFactor`** (conceptual): compares a point's local density to its neighbors'
  local density; points in a much sparser neighborhood than their neighbors are flagged as
  outliers. Useful when "normal" density varies across regions of the feature space (unlike a
  single global threshold).

## 6. In-Class Exercise
Given a scree plot of cumulative explained variance, determine the minimum number of components
needed to retain at least 90% of the variance, and discuss one tradeoff of choosing fewer vs.
more components.
