# Week 12 Summary — Dimensionality Reduction Theory

**Key takeaways:**
- PCA's variance-maximization objective, solved via a Lagrangian, shows the optimal projection
  directions are exactly the eigenvectors of the covariance matrix, ranked by eigenvalue.
- The Courant–Fischer theorem (with an inductive deflation sketch) proves the top-$k$ eigenvectors
  are jointly optimal — not a greedy approximation — among all rank-$k$ orthogonal projections.
- Kernel PCA performs PCA implicitly in an RKHS by eigendecomposing a centered kernel matrix,
  capturing nonlinear structure linear PCA cannot.
- Isomap (geodesic distances) and t-SNE (local-probability matching with a heavy-tailed embedding)
  go further still for nonlinear manifold structure, at the cost of a simple out-of-sample
  extension or globally meaningful distances.

**You should now be able to:** prove PCA's optimality via eigendecomposition and implement both
PCA and kernel PCA from scratch.

**Next week:** causal inference basics — correlation vs. causation, confounding, and Simpson's
paradox.
