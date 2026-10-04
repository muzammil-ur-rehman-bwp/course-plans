# Lab Notes 2 — Linear Regression From Scratch and with scikit-learn

**Concept recap:** gradient descent and the normal equation both minimize the MSE cost function
and should converge to (nearly) the same `theta` for ordinary least squares.

**Common pitfalls:**
- Forgetting to add the intercept (bias) column of 1s before matrix-multiplying by `theta` in the
  from-scratch implementation, which silently fits a model with no intercept.
- Using too large a learning rate, causing the cost to oscillate or diverge instead of decreasing.
- Not scaling features before gradient descent when features are on very different scales — this
  can make convergence extremely slow even with a reasonable learning rate (full treatment in
  Week 3).

**Debugging tip:** if the cost curve from gradient descent increases or oscillates, the first fix
to try is lowering the learning rate by a factor of 10; if it decreases but very slowly, try
scaling features or increasing `n_iters`.

**Instructor tip:** have students deliberately set `alpha` too high (e.g., 10x a working value)
and plot the resulting cost curve, so the failure mode of divergence is seen directly rather than
only described.
