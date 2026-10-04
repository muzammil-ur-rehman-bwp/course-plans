# Week 14 Summary — Model Selection Theory

**Key takeaways:**
- The bias-variance decomposition $\mathbb{E}[(y-\hat h(x))^2]=\mathrm{Bias}[\hat
  h(x)]^2+\mathrm{Var}[\hat h(x)]+\sigma^2$ follows from two clean expectation splits (noise, then
  bias/variance), with every cross term vanishing by independence or by definition of the mean.
- AIC's $2k$ penalty approximates the expected optimism bias of in-sample log-likelihood (an
  asymptotic KL-divergence argument); BIC's $k\ln n$ penalty comes from a Laplace approximation to
  the Bayesian marginal likelihood, and grows with $n$, making BIC asymptotically model-selection
  consistent while AIC targets predictive optimality.
- Cross-validation is theoretically justified as an (approximately unbiased, if $k$-fold) estimator
  of generalization risk for the specific $\hat h$ produced — complementary to, not competing
  with, the Weeks 2–5 worst-case PAC/VC/Rademacher bounds.

**You should now be able to:** prove the bias-variance decomposition, compute AIC/BIC for nested
models, and explain cross-validation's theoretical justification.

**Next week:** research methods — reading and critiquing theoretical ML papers, reproducibility,
and capstone work time.
