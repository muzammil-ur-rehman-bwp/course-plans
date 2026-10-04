# Lab Notes 12 — PCA and Kernel PCA From Scratch

**Concept recap:** PCA's top-$k$ eigenvectors are the provably optimal rank-$k$ variance-maximizing
projection; kernel PCA performs the same optimization implicitly in an RKHS via a centered kernel
matrix; Isomap and t-SNE go further for strongly nonlinear manifolds, at the cost of losing PCA's
simple linear out-of-sample extension.

**Common pitfalls:**
- Comparing from-scratch PCA components directly to `sklearn.decomposition.PCA`'s without
  accounting for the arbitrary sign flip each eigenvector carries (an eigenvector $u$ and $-u$ are
  equally valid) — compare via correlation magnitude, not raw values, as instructed.
- **Forgetting that the kernel must be positive semi-definite** when implementing kernel PCA's
  centered kernel matrix from scratch — a centering-step bug (wrong matrix dimensions or a
  transpose error in $\mathbf{1}_mK\mathbf{1}_m$) can silently produce a non-PSD matrix whose
  eigendecomposition still runs but produces meaningless negative "eigenvalues" for components
  that should be exactly zero.
- Expecting t-SNE's axes or distances to be directly interpretable the way PCA's are — t-SNE
  preserves *local* neighborhood structure, not global distances or absolute scale; two clusters'
  distance apart in a t-SNE plot carries no quantitative meaning.

**Debugging tip:** if kernel PCA fails to separate the concentric circles in Task B, check the RBF
kernel's `gamma` value first — too small a `gamma` makes the kernel nearly constant (uninformative
Gram matrix); too large makes it nearly diagonal (every point looks equally dissimilar to every
other).

**Instructor tip:** have students explicitly verify that linear PCA's leading component on the
Swiss-roll data (Task C) captures mostly the roll's *radius* direction, not its unrolled
intrinsic coordinate — a concrete, visual counter-example to "PCA always finds the interesting
structure."
