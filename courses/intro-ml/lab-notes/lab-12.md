# Lab Notes 12 — Clustering

**Concept recap:** k-means needs `k` chosen in advance and scaled features; the elbow method and
silhouette score both help choose `k`, and agglomerative clustering gives a full hierarchy via a
dendrogram.

**Common pitfalls:**
- Running k-means on unscaled features, letting one large-range feature dominate the distance
  calculation and distort the clusters.
- Choosing `k` purely by the elbow plot when the "elbow" is ambiguous — cross-checking with the
  silhouette score (which can be optimized numerically, unlike eyeballing an elbow) gives a more
  defensible choice.
- Forgetting that k-means and silhouette scores depend on `random_state`/initialization; use
  `n_init` greater than 1 (scikit-learn's default already reruns multiple times) and do not
  over-interpret a single run on borderline cases.

**Debugging tip:** if silhouette scores are low (near 0) for every `k` tried, the data may not
have well-separated cluster structure at all — consider whether clustering is the right tool for
this dataset, or whether dimensionality reduction (Week 13) might reveal structure not visible in
the original feature space.

**Instructor tip:** have students run k-means with `n_init=1` and a few different `random_state`
values on a deliberately ambiguous dataset, to see how initialization can change the result when
not using a robust multi-restart strategy.
