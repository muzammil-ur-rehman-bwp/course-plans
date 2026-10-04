# Lab Notes 9 — Bayesian Linear Regression From Scratch

**Concept recap:** the Bayesian linear regression posterior is Gaussian in closed form; its mode
(MAP estimate) equals the ridge-regression solution for $\lambda=\sigma^2/\tau^2$; the posterior
covariance gives uncertainty estimates ordinary ridge regression does not provide.

**Common pitfalls:**
- Mismatching $\lambda$ and $\sigma^2/\tau^2$ when comparing against `sklearn.linear_model.Ridge`
  — `Ridge`'s `alpha` parameter *is* $\lambda$ directly (no extra scaling by $m$, unlike kernel
  ridge regression in Lab 7); double-check this convention before concluding the two don't match.
- Treating $\sigma^2$ (assumed known in this derivation) as a free parameter to tune for best
  fit — in this closed-form derivation it is a modeling assumption about the noise level, not a
  regularization knob; conflating the two misreads what the posterior covariance actually
  represents.
- Reporting only the MAP point estimate and ignoring `Sigma_N` entirely — the whole pedagogical
  point of Task C/D is that the posterior gives you calibrated uncertainty for free, which a plain
  ridge-regression fit does not.

**Debugging tip:** if the posterior standard deviations in Task C do not shrink as $n$ grows,
check that `X` is being redrawn with *more rows*, not just re-fit on the same fixed small sample
repeatedly.

**Instructor tip:** use Task E's prior-sensitivity sweep to connect concretely back to Week 1's
overfitting lesson: an extremely loose prior ($\tau^2\to\infty$) should visibly reduce to
unregularized least squares, and an extremely tight one ($\tau^2\to0$) should visibly shrink all
coefficients toward zero regardless of the data.
