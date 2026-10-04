# Week 10 Summary — Bayesian Machine Learning II: Gaussian Processes

**Key takeaways:**
- A Gaussian Process is a prior over functions such that any finite set of function values is
  jointly Gaussian; a positive-definite kernel (Week 7) is exactly what guarantees this is
  well-defined.
- The GP regression predictive mean and covariance follow from standard Gaussian conditioning
  applied to the joint distribution of training and test outputs — a closed-form, exact posterior,
  not an approximation.
- The predictive mean takes the same kernel-expansion form the representer theorem guarantees;
  the predictive covariance shrinks near observed data and reverts to the prior away from it.
- `scipy.linalg.cho_factor`/`cho_solve` (Cholesky) is used instead of direct matrix inversion for
  numerical stability when $K$ is near-singular.

**You should now be able to:** derive the GP regression predictive equations and implement GP
regression from scratch with a numerically stable solve.

**Next week:** structured prediction — Conditional Random Fields.
