# Week 8 Summary — Regularization Theory; Midterm Review

**Key takeaways:**
- The classical bias-variance view predicts a U-shaped test-error-vs-capacity curve with
  monotonic overfitting past the optimum; double descent shows test error falling again once
  capacity exceeds the interpolation threshold, a robust empirical phenomenon any complete theory
  must explain.
- Weight decay is exactly MAP estimation under a zero-mean Gaussian prior on weights, with
  $\lambda = 1/\tau^2$.
- Dropout can be read as approximately averaging over an implicit ensemble of subnetworks, linking
  it conceptually to Bayesian model averaging.

**You should now be able to:** reproduce a double-descent curve empirically, derive weight decay
as MAP estimation, and explain dropout's Bayesian-averaging interpretation.

**Next week:** Midterm Exam (Weeks 1–8), then generalization theory I — PAC learning and VC
dimension.
