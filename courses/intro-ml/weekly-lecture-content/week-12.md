# Week 12 — Lecture Content: Unsupervised Learning I — Clustering

## 1. The k-Means Algorithm
k-means partitions `n` points into `k` clusters by minimizing the within-cluster sum of squared
distances to each cluster's centroid:
```
J = sum_{i=1}^{n} || x_i - mu_{c(i)} ||^2
```
where `c(i)` is the cluster assigned to point `i` and `mu_c` is cluster `c`'s centroid. It
alternates two steps until convergence:
1. **Assign step:** assign each point to its nearest centroid.
2. **Update step:** recompute each centroid as the mean of the points assigned to it.
This is guaranteed to decrease `J` (or leave it unchanged) at every step, so it converges, but
only to a **local** minimum — initialization matters, which is why scikit-learn's default
`k-means++` initialization spreads initial centroids apart, and `n_init` reruns from multiple
initializations and keeps the best result.

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=3, n_init=10, random_state=42)
labels = kmeans.fit_predict(X_scaled)   # scaling matters: k-means is distance-based
```
**Feature scaling is required**, exactly as for k-NN and SVMs, since k-means clusters by
Euclidean distance.

## 2. Choosing k: The Elbow Method
k-means requires `k` to be specified in advance. The **elbow method** plots **inertia** (`J`
above, available as `kmeans.inertia_`) against `k`; inertia always decreases as `k` increases
(more clusters can only fit the data at least as well), but the rate of decrease typically slows
sharply at some `k` — the "elbow" — suggesting a reasonable number of clusters.
```python
import matplotlib.pyplot as plt

inertias = []
for k in range(1, 10):
    inertias.append(KMeans(n_clusters=k, n_init=10, random_state=42).fit(X_scaled).inertia_)

plt.plot(range(1, 10), inertias, marker="o")
plt.xlabel("k"); plt.ylabel("Inertia"); plt.title("Elbow plot")
plt.show()
```

## 3. Hierarchical (Agglomerative) Clustering
Agglomerative clustering starts with every point as its own cluster and repeatedly merges the
two closest clusters until one cluster remains, building a tree of merges (a **dendrogram**).
"Closest" depends on the **linkage criterion**:
- **Single linkage:** minimum distance between any pair of points in the two clusters.
- **Complete linkage:** maximum such distance.
- **Average linkage:** average such distance.
- **Ward linkage:** merges that minimize the resulting increase in within-cluster variance
  (scikit-learn's default, and a reasonable general-purpose choice).

```python
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage
import matplotlib.pyplot as plt

agg = AgglomerativeClustering(n_clusters=3, linkage="ward")
agg_labels = agg.fit_predict(X_scaled)

Z = linkage(X_scaled, method="ward")
dendrogram(Z)
plt.title("Agglomerative clustering dendrogram")
plt.show()
```
Unlike k-means, agglomerative clustering does not require `k` to be fixed in advance — the
dendrogram can be "cut" at any height to yield a chosen number of clusters, and the whole
hierarchy is visible at once.

## 4. Evaluating Clusters: The Silhouette Score
With no ground-truth labels, the **silhouette score** measures how well each point fits its
assigned cluster relative to the next-nearest cluster. For point `i`:
```
a(i) = mean distance from i to other points in its own cluster      (cohesion)
b(i) = mean distance from i to points in the nearest other cluster  (separation)
s(i) = (b(i) - a(i)) / max(a(i), b(i))
```
`s(i)` ranges from -1 (likely misclassified) to +1 (well clustered); the overall silhouette
score is the mean of `s(i)` over all points. It can compare **different numbers of clusters**,
unlike inertia (which mechanically favors more clusters).
```python
from sklearn.metrics import silhouette_score

for k in range(2, 8):
    labels = KMeans(n_clusters=k, n_init=10, random_state=42).fit_predict(X_scaled)
    print(k, silhouette_score(X_scaled, labels))
```

## 5. When to Use Which
k-means assumes roughly spherical, similarly-sized clusters and scales well to large datasets but
needs `k` chosen in advance. Agglomerative clustering makes fewer shape assumptions and gives a
full hierarchy for free, but scales poorly to very large datasets (naively `O(n^2)` or worse) and
still requires choosing a cut point (number of clusters) in the end.

## 6. In-Class Exercise
On a toy 2D dataset, run k-means for `k` in `{2,3,4,5}`; plot the elbow curve and compute the
silhouette score at each `k`; determine whether both methods agree on the best `k`.
