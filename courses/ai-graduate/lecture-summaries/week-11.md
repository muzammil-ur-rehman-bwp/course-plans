# Week 11 Summary — Probabilistic Graphical Models at Rigor

**Key takeaways:**
- Exact inference in general Bayesian networks is #P-hard; even variable elimination's
  worst-case cost is exponential in the network's treewidth.
- Rejection sampling discards samples inconsistent with evidence, wasting computation when
  evidence is rare; likelihood weighting fixes evidence variables and weights each sample by
  evidence likelihood instead, using every sample.
- MCMC/Gibbs sampling builds a Markov chain over assignments that converges to the true
  posterior without rejection or global weighting, at the cost of needing a burn-in period
  (covered only conceptually).

**You should now be able to:** explain why exact inference is worst-case intractable; implement
rejection sampling and likelihood weighting; compare their efficiency empirically.

**Next week:** The computational complexity of AI problems — NP-completeness of SAT/CSP and
PSPACE-completeness of planning, unified and revisited.
