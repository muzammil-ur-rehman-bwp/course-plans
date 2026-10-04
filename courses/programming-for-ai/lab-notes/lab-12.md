# Lab Notes 12 — Clustering & PCA

**Concept recap:** k-means iterates assign→update until convergence; the elbow method trades off
inertia reduction against added complexity (more clusters); PCA retains the directions of
greatest variance, useful for visualization and noise reduction.

**Common pitfalls:**
- Not scaling features before k-means — features with larger numeric ranges dominate the
  distance calculation and skew clustering results.
- Reading too much into an ambiguous "elbow" — real datasets often don't have a crisp elbow;
  justify the choice qualitatively rather than searching for a nonexistent sharp corner.
- Confusing PCA components with "the two most important original features" — components are
  linear combinations of all original features, not a subset of them.

**Debugging tip:** always scale (`StandardScaler`) before k-means/PCA unless there's a specific
reason not to; compare clustering with and without scaling to see the effect directly.

**Instructor tip:** if the dataset has hidden/removed labels (e.g., label-stripped Iris), reveal
them at the end so students can check how well their k-means clusters matched the true classes.
