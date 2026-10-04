# Lab Notes 12 — Median-of-Means and Trimmed-Mean Robust Estimation

**Concept recap:** median-of-means partitions data into $k=O(\log(1/\delta))$ groups and takes
the median of group means, achieving sub-Gaussian-type error under only a finite-variance
assumption; the trimmed mean discards the extreme $\epsilon$-fraction of order statistics,
with breakdown point $\epsilon$.

**Common pitfalls:**
- Choosing $k$ too large relative to $n$ in median-of-means — if $k$ is so large that group size
  $m=n/k$ becomes tiny, each group mean's own Chebyshev concentration (Step 1 of the derivation)
  becomes weak, and the overall bound's $\sigma\sqrt{k/n}$ term grows even as the majority-vote
  boosting (Step 2) improves — there is a real tradeoff, not "more groups is always better."
- In `trimmed_mean`, trimming by *count* (`int(floor(eps*n))`) versus trimming by exact fraction
  can differ slightly at small $n$ — be explicit and consistent about which convention you use,
  and do not expect the breakdown point to be observed with mathematical exactness at finite,
  small $n$.
- In Task D, using the *same* fixed random corruption indices across different $\epsilon$ values
  by accident (e.g., caching an index array) — regenerate corrupted indices fresh for each
  $\epsilon$, since the whole point is varying the corrupted fraction.

**Debugging tip:** as a sanity check, run `median_of_means` with `k=1` and confirm it reduces
exactly to the ordinary sample mean (a single group's "median" is just its own mean) — a quick way
to catch an indexing bug in `np.array_split`.

**Instructor tip:** ask students, before Task D, to predict at what $\epsilon$ the trimmed mean's
error curve will visibly depart from its low, flat baseline — most predict correctly that it is
near $\epsilon=0.1$ (its coded breakdown point), which is a satisfying, directly-predictable
result once the theory is understood.
