# Week 10 Summary — Distribution Shift and Domain Adaptation

**Key takeaways:**
- Covariate shift assumes $P(y|x)$ is preserved while $P(x)$ shifts; importance weighting,
  $\mathbb{E}_{P_{\mathrm{te}}}[\ell] = \mathbb{E}_{P_{\mathrm{tr}}}[w(x)\ell]$, corrects for it
  exactly, derived directly from the definition of expectation.
- The density ratio $w(x)$ can be estimated via a domain classifier's odds, avoiding direct
  high-dimensional density estimation.
- Generalization guarantees under shift pick up a divergence penalty that blows up under poor
  overlap — a structural, not merely implementational, limit on domain adaptation.

**You should now be able to:** derive the importance-weighted risk identity; estimate density-
ratio weights via a classifier; explain why poor overlap causes high-variance weights and when
domain-adaptation guarantees become vacuous.

**Next week:** Algorithmic fairness — formal fairness criteria and the impossibility results
governing their simultaneous satisfaction.
