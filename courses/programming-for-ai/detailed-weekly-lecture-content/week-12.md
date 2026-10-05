# Week 12: Unsupervised Learning

## Learning Objectives

By the end of this lecture, you should be able to:

1. Describe the k-means algorithm and implement it from scratch.
2. Use the elbow method, and the silhouette score, to choose the number of clusters.
3. Explain what principal component analysis (PCA) does and read the explained variance ratio.
4. Combine k-means and PCA to inspect a clustering in two dimensions.
5. State the assumptions and limits of k-means.

## 1. Learning Without Labels

Everything in Weeks 10 and 11 relied on labelled data. Often we do not have labels. A shop has records of what customers buy, but nobody has sorted the customers into types. A biologist has gene measurements for thousands of cells, with no names for the cell types. Unsupervised learning looks for structure in such data on its own.

The two tasks we study are clustering, which groups similar points, and dimensionality reduction, which finds a smaller description of the data. There is no test accuracy, because there is no correct answer to compare with, and that makes judgement and care more important, not less.

## 2. k-Means Clustering

k-means partitions the data into `k` clusters. The idea is simple and is described by a short loop.

1. Pick `k` initial centroids, for example `k` random data points.
2. Assign each point to its nearest centroid.
3. Recompute each centroid as the mean of the points assigned to it.
4. Repeat steps 2 and 3 until the assignments stop changing, or a maximum number of iterations is reached.

Each round can only lower, or keep equal, the total squared distance of the points to their centroids, so the loop always terminates. It does not guarantee the best possible clustering, only a local optimum, and this has consequences we discuss below.

### 2.1 k-means from scratch

Writing it out once makes the algorithm concrete. We use NumPy broadcasting, as in Week 3, to compute all the distances at once.

```python
import numpy as np

def kmeans(X, k, n_iter=100, seed=0):
    rng = np.random.default_rng(seed)
    centroids = X[rng.choice(len(X), size=k, replace=False)]
    for _ in range(n_iter):
        # distances has shape (n_points, k)
        distances = np.linalg.norm(X[:, None, :] - centroids[None, :, :], axis=2)
        labels = distances.argmin(axis=1)
        new_centroids = np.array([
            X[labels == j].mean(axis=0) if np.any(labels == j) else centroids[j]
            for j in range(k)
        ])
        if np.allclose(new_centroids, centroids):
            break
        centroids = new_centroids
    inertia = ((X - centroids[labels]) ** 2).sum()
    return labels, centroids, inertia
```

The expression `X[:, None, :] - centroids[None, :, :]` inserts extra axes so that the subtraction broadcasts to a `(n_points, k, n_features)` array. This is the same technique we used for distances to many points. Test it on three blobs of points.

```python
from sklearn.datasets import make_blobs

X, y_true = make_blobs(n_samples=300, centers=3, cluster_std=1.0, random_state=42)
labels, centroids, inertia = kmeans(X, k=3)
print("centroids:\n", centroids.round(2))
print("cluster sizes:", np.bincount(labels))
print("inertia:", round(inertia, 1))
```

### 2.2 scikit-learn

```python
from sklearn.cluster import KMeans

km = KMeans(n_clusters=3, n_init=10, random_state=42)
labels_sk = km.fit_predict(X)
print(km.cluster_centers_.round(2))
print("inertia:", round(km.inertia_, 1))
```

The `n_init=10` argument runs the algorithm ten times with different starting centroids and keeps the best result. This addresses the main weakness, which is that the outcome depends on where the centroids start. The scikit-learn version also uses a smarter initialization called k-means++, which spreads the starting points out.

You can see the sensitivity to initialization with our own function.

```python
for seed in range(6):
    _, _, inertia = kmeans(X, k=3, seed=seed)
    print(f"seed {seed}: inertia {inertia:.1f}")
```

On our run, seeds 2, 3 and 4 give an inertia of about 567, which is the right answer, while seeds 0, 1 and 5 give about 5,694. Those runs started with two centroids inside the same blob, so one blob was split in two and two other blobs were merged. The algorithm converged happily to a poor answer. This is why we restart several times and keep the best result.

### 2.3 Assumptions and limits

k-means is a strong baseline, but it is not universally appropriate. It assumes that:

1. Clusters are roughly round, since it uses Euclidean distance to a centre.
2. Clusters have similar sizes and spreads.
3. All features are on comparable scales. Scale the data first, as in Week 10.
4. The number of clusters `k` is known in advance.

A classic failure uses two interleaved half moons.

```python
from sklearn.datasets import make_moons
from sklearn.metrics import adjusted_rand_score

Xm, ym = make_moons(n_samples=400, noise=0.06, random_state=0)
lm = KMeans(n_clusters=2, n_init=10, random_state=0).fit_predict(Xm)
print("agreement with true moons (1.0 is perfect):", round(adjusted_rand_score(ym, lm), 3))
```

The score is far from 1, because k-means cuts the moons with a straight line rather than following their shapes. Methods based on density, such as DBSCAN, handle such shapes. The lesson is that the algorithm's assumptions have to match the data.

## 3. Choosing k

Often `k` is not known. Inertia, the sum of squared distances from points to their centroid, always decreases as `k` grows, so we cannot just minimize it. At `k` equal to the number of points it is zero. The elbow method looks for the value of `k` after which adding clusters gives only small gains.

```python
import matplotlib.pyplot as plt

inertias = []
ks = range(1, 10)
for k in ks:
    km = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X)
    inertias.append(km.inertia_)

for k, val in zip(ks, inertias):
    print(f"k={k}  inertia={val:9.1f}")

plt.plot(list(ks), inertias, marker="o", color="black")
plt.xlabel("number of clusters k")
plt.ylabel("inertia")
plt.title("Elbow plot")
plt.show()
```

For our three blobs, the drop is steep from 1 to 2 to 3 clusters and then flattens. That bend at `k = 3` is the elbow. On real data the elbow is often vague, and picking it is partly a matter of judgement.

A second tool is the silhouette score. For each point it compares the average distance to its own cluster with the average distance to the nearest other cluster, giving a number between -1 and 1. Higher is better, and the mean over all points can be compared across values of `k`.

```python
from sklearn.metrics import silhouette_score

for k in range(2, 8):
    lab = KMeans(n_clusters=k, n_init=10, random_state=42).fit_predict(X)
    print(f"k={k}  silhouette={silhouette_score(X, lab):.3f}")
```

The silhouette score peaks at the best `k`, here 3, with a value of 0.85, which signals very well separated clusters. Using both tools, and then asking whether the resulting groups make sense for the application, is better than trusting either one blindly.

## 4. Principal Component Analysis

Real data often has many features, and we cannot plot more than two or three dimensions. PCA finds new axes, called principal components, ordered by how much of the variation in the data they capture. The first component is the direction of largest spread, the second is the direction of largest remaining spread at right angles to the first, and so on. Keeping only the first few components gives a compressed description that loses as little information as possible.

Mathematically, the components are the eigenvectors of the covariance matrix, which connects PCA to the linear algebra of Week 3.

```python
from sklearn.decomposition import PCA
from sklearn.datasets import load_iris

iris = load_iris()
Xi = iris.data
pca = PCA(n_components=2)
Xi_2d = pca.fit_transform(Xi)

print("shape before:", Xi.shape, " after:", Xi_2d.shape)
print("explained variance ratio:", pca.explained_variance_ratio_.round(3))
print("total kept:", pca.explained_variance_ratio_.sum().round(3))
```

The `explained_variance_ratio_` shows the fraction of the total variance each component retains. For the iris data, two components keep about 98 percent, so a two dimensional plot loses very little. To decide how many components are enough, fit PCA with all components and look at the cumulative sum.

```python
full = PCA().fit(Xi)
print("cumulative:", np.cumsum(full.explained_variance_ratio_).round(3))
```

Two practical points. First, PCA is sensitive to scale, because a feature with large numbers would dominate the variance, so standardize before applying it unless all features share a unit. Second, the new axes are mixtures of the original features, which makes them harder to interpret.

```python
print("component 1 weights:", dict(zip(iris.feature_names, pca.components_[0].round(2))))
```

## 5. Putting It Together

A standard pattern is to cluster in the original feature space and then use PCA only to draw the picture. We colour the points by their cluster label.

```python
from sklearn.preprocessing import StandardScaler

Xs = StandardScaler().fit_transform(Xi)
clusters = KMeans(n_clusters=3, n_init=10, random_state=0).fit_predict(Xs)
coords = PCA(n_components=2).fit_transform(Xs)

plt.scatter(coords[:, 0], coords[:, 1], c=clusters, cmap="gray")
plt.xlabel("principal component 1")
plt.ylabel("principal component 2")
plt.title("k-means clusters of the iris data, shown with PCA")
plt.show()
```

Since iris does come with species labels, we can use them to judge the clustering after the fact. This is an unusual luxury, and it is only for teaching.

```python
import pandas as pd

table = pd.crosstab(pd.Series(iris.target_names[iris.target], name="species"),
                    pd.Series(clusters, name="cluster"))
print(table)
print("adjusted Rand index:", round(adjusted_rand_score(iris.target, clusters), 3))
```

One species, setosa, is perfectly separated into its own cluster. The other two overlap, and k-means mixes some of them. This is consistent with what the PCA scatter plot shows. The clustering found real structure, but not exactly the human categories, and that is normal.

## 6. In-Class Exercise

Run k-means with k = 2, 3, 4, 5 on a provided dataset, plot the elbow curve, justify a choice of `k`, and visualize the chosen clustering with PCA.

A runnable version on the wine dataset, which has 13 chemical measurements:

```python
from sklearn.datasets import load_wine

wine = load_wine()
Xw = StandardScaler().fit_transform(wine.data)

print(" k  inertia  silhouette")
for k in [2, 3, 4, 5]:
    km = KMeans(n_clusters=k, n_init=10, random_state=0).fit(Xw)
    print(f"{k:2d}  {km.inertia_:7.1f}  {silhouette_score(Xw, km.labels_):.3f}")

best = KMeans(n_clusters=3, n_init=10, random_state=0).fit(Xw)
coords = PCA(n_components=2).fit_transform(Xw)
print("PCA kept:", PCA(n_components=2).fit(Xw).explained_variance_ratio_.sum().round(3))

plt.scatter(coords[:, 0], coords[:, 1], c=best.labels_, cmap="gray")
plt.title("Wine data: k-means with k=3, shown in PCA space")
plt.show()
```

Write a short justification, for example: "The inertia falls steadily with no sharp elbow, with the largest drop from k=2 to k=3, and the silhouette score is highest at k=3, although only 0.29, which means the clusters overlap. The wine data is known to come from three grape varieties, so three clusters also makes sense in the application. The two PCA components keep only 55 percent of the variance, so the plot is an approximate picture."

Questions:

1. What happens to the results if you skip the standardization? Try it.
2. Does `adjusted_rand_score(wine.target, best.labels_)` show good agreement with the real grape labels?
3. Why would it be wrong to call the clusters "correct" simply because the algorithm produced them?

## 7. Common Mistakes

1. Running k-means on unscaled features.
2. Trusting a single random initialization.
3. Reading the elbow plot too confidently when there is no clear elbow.
4. Interpreting PCA axes as if they were original features.
5. Using PCA on unscaled data with mixed units.
6. Treating cluster labels as ground truth, instead of as a hypothesis to check with domain knowledge.

## 8. Summary

Unsupervised learning finds structure without labels. k-means alternates between assigning points and updating centres, and it works well for compact round clusters on scaled features, but depends on its starting point and on the choice of `k`. The elbow and silhouette are helpful aids to choosing `k`. PCA compresses features onto directions of greatest variance, which makes high-dimensional data visible. Together they form a standard way to explore a new dataset.

## 9. Practice Problems

1. Modify the `kmeans` function to use k-means++ initialization and compare the spread of inertia across seeds.
2. Apply PCA to the 64 pixel features of `sklearn.datasets.load_digits`, and find how many components retain 90 percent of the variance.
3. Cluster the digits data into 10 clusters, and compare with the true digit labels using a crosstab.
4. Run DBSCAN on the half moons data and compare its result with k-means.

## 10. Suggested Reading

1. The scikit-learn user guide sections on clustering and on decomposition.
2. Géron, Hands-On Machine Learning, the chapter on unsupervised learning.
