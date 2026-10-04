# Week 4 Summary — High-Dimensional Statistics II: Random Matrix Theory Basics

**Key takeaways:**
- The Marchenko–Pastur law describes the limiting empirical spectral distribution of a sample
  covariance matrix when $p/n\to\gamma$, with support $[(1-\sqrt\gamma)^2,(1+\sqrt\gamma)^2]$.
- Even with true covariance $=I$, sample eigenvalues spread across this entire interval — a
  structural artifact of high dimensionality, not real signal.
- As $\gamma\to 0$, the classical regime is recovered; as $\gamma\to1$, the smallest eigenvalue
  approaches 0 and $\hat\Sigma$ becomes nearly singular.

**You should now be able to:** state the Marchenko–Pastur law and its support formula; explain
why naive high-dimensional PCA/covariance-eigenstructure claims require checking against the
Marchenko–Pastur null spectrum; simulate and confirm the law empirically.

**Next week:** Full-information online convex optimization — the OCO protocol, Follow-The-
Regularized-Leader, and online gradient descent's regret bound, derived.
