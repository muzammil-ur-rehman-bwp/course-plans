# Week 12 — Lecture Content: Unsupervised Learning

## 1. k-Means Clustering
Partitions data into `k` clusters by iterating:
1. Assign each point to the nearest of `k` centroids.
2. Recompute each centroid as the mean of points assigned to it.
3. Repeat until assignments stop changing (or max iterations reached).
```python
from sklearn.cluster import KMeans

km = KMeans(n_clusters=3, random_state=42)
labels = km.fit_predict(X)
print(km.cluster_centers_)
```
k-means is sensitive to the initial centroid placement and assumes roughly spherical, similarly
sized clusters — it is a strong baseline but not universally appropriate.

## 2. Choosing k: The Elbow Method
Plot within-cluster sum of squares (inertia) against `k`; look for the "elbow" where additional
clusters stop meaningfully reducing inertia.
```python
inertias = []
for k in range(1, 10):
    km = KMeans(n_clusters=k, random_state=42).fit(X)
    inertias.append(km.inertia_)
# plt.plot(range(1, 10), inertias)
```

## 3. Principal Component Analysis (PCA)
Reduces dimensionality by projecting data onto the directions (principal components) that
capture the most variance, enabling visualization of high-dimensional data in 2D/3D.
```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X)
print(pca.explained_variance_ratio_)
```
`explained_variance_ratio_` tells us how much of the original variance each component retains —
useful for deciding how many components are "enough."

## 4. Putting It Together
Cluster a dataset with k-means, then use PCA to reduce it to 2D for visualization, coloring
points by their assigned cluster label — a standard pattern for inspecting clustering results.

## 5. In-Class Exercise
Run k-means with k = 2, 3, 4, 5 on a provided dataset, plot the elbow curve, justify a choice of
k, and visualize the chosen clustering via PCA.
