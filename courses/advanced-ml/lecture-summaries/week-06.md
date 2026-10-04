# Week 6 Summary — Nonparametric Bayesian Methods I: The Dirichlet Process

**Key takeaways:**
- A Dirichlet process $\mathrm{DP}(\alpha,H)$ is a prior over distributions, defined by: for every
  finite partition, the induced probability vector is Dirichlet-distributed with parameters
  $\alpha H(A_i)$.
- The stick-breaking construction ($\beta_k\sim\mathrm{Beta}(1,\alpha)$, $\pi_k=\beta_k\prod_{j<k}
  (1-\beta_j)$, atoms $\theta_k\sim H$) gives an explicit, almost-surely-discrete draw whose
  weights provably sum to 1.
- The concentration parameter $\alpha$ controls the effective number of clusters: small $\alpha$
  concentrates mass on few atoms; large $\alpha$ spreads it thinly, approaching $H$.

**You should now be able to:** state the DP's defining property; derive the stick-breaking
weight-sum-to-1 argument; implement a truncated stick-breaking sampler and relate $\alpha$ to
cluster concentration.

**Next week:** Nonparametric Bayesian methods II — the Chinese Restaurant Process and infinite
mixture models.
