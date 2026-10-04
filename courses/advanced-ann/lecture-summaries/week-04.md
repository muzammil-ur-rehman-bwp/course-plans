# Week 4 Summary — Implicit Regularization and the Implicit Bias of Gradient Descent

**Key takeaways:**
- Implicit regularization names whatever property of the optimization algorithm (not the loss)
  selects one particular solution among many that fit the training data.
- On linearly separable data with an exponential-tailed loss (e.g., logistic), gradient descent
  with no explicit regularizer converges in *direction* to the $L_2$ max-margin (hard-SVM)
  classifier, with the norm growing only logarithmically in the number of steps (Soudry et al.).
- The general nonlinear/deep case is only partially understood: deep-linear/matrix-factorization
  settings show a (different) bias toward low-rank/simple solutions, but no comparably clean
  general characterization exists for genuinely nonlinear deep networks — an open research area.

**You should now be able to:** derive the max-margin implicit-bias result's key steps; predict
which loss functions should/should not produce the same bias from their tail behavior; state
precisely what is settled versus open once linearity is dropped.

**Next week:** Feature learning beyond the kernel regime — how finite-width networks escape the
NTK/lazy dynamics of Week 2, and why this matters for explaining deep learning's empirical success.
