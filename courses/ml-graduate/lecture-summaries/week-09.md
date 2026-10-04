# Week 9 Summary — Midterm Exam; Bayesian Machine Learning I

**Key takeaways:**
- Completing the square in the log-posterior shows that, for a Gaussian prior and Gaussian
  likelihood, the posterior over weights is exactly Gaussian, with closed-form mean $\mu_N$ and
  covariance $\Sigma_N$.
- The MAP estimate (the posterior mode, which equals the mean for a Gaussian) is *exactly* the
  ridge regression solution, with $\lambda=\sigma^2/\tau^2$ — regularization strength is the
  ratio of noise variance to prior variance.
- The posterior predictive distribution's variance has two sources: parameter uncertainty
  (shrinks with more data) and irreducible observation noise (never shrinks) — a preview of
  Week 14's bias-variance decomposition.

**You should now be able to:** derive the Bayesian linear regression posterior in closed form and
explain ridge regression as MAP estimation.

**Next week:** Gaussian Processes — extending this Bayesian view from a finite weight vector to a
full prior over functions.
