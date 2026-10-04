# Lab Notes 14 — Bias-Variance Decomposition and Information Criteria

**Concept recap:** the bias-variance decomposition splits squared-error risk into systematic
error (bias²), sensitivity to the training sample (variance), and irreducible noise ($\sigma^2$);
AIC and BIC penalize model complexity differently (fixed $2k$ vs. growing $k\ln n$), and
cross-validation estimates generalization risk directly and empirically rather than via an
asymptotic approximation.

**Common pitfalls:**
- Using too few resamples (`n_resamples`) when estimating bias and variance empirically — with
  too few resamples, the measured variance is itself a noisy estimate, which can make Task B's
  "bias²+variance+σ² ≈ measured MSE" check fail to match closely even when the implementation is
  correct; use at least a few hundred resamples.
- Computing AIC/BIC with a log-likelihood from the wrong noise model (e.g., silently assuming unit
  variance instead of estimating $\hat\sigma^2$ from residuals) — this shifts every model's
  log-likelihood by the same amount in some cases but not others, and can change which model looks
  best.
- Treating AIC/BIC and cross-validation as if they must always agree — they are different
  estimators with different theoretical targets (Week 14, Section 5); a mismatch between what
  Task D and Task E select is an expected, discussable result, not a sign of a bug.

**Debugging tip:** if Task A's bias² curve does not decrease monotonically with degree, check
that `fit_polynomial` is not accidentally underfitting due to numerical conditioning issues at
high degree (very high-degree `np.polyfit` can become ill-conditioned) — consider centering/scaling
`x` first if this occurs.

**Instructor tip:** use Task D/E's side-by-side comparison to reinforce Week 14's framing
explicitly: ask students which criterion (AIC, BIC, or CV) they would trust most if their actual
goal were *prediction* on new data vs. *identifying the true underlying model*, and why.
