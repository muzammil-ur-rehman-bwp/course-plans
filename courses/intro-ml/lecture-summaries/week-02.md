# Week 2 Summary — Linear Regression in Depth

**Key takeaways:**
- Linear regression fits `y_hat = X @ theta` by minimizing the MSE cost function `J(theta)`.
- Gradient descent iteratively updates `theta` opposite the gradient of `J`; the normal equation
  solves for the optimal `theta` directly via `(X^T X)^{-1} X^T y`.
- Both approaches minimize the same convex cost function and converge to the same solution for
  ordinary least squares; gradient descent scales better to very large feature counts.
- Linear regression assumes linearity, independent and homoscedastic residuals, and
  (for inference) approximately normal residuals; residual plots help diagnose violations.

**You should now be able to:** derive and implement gradient descent for linear regression from
scratch; fit and evaluate `LinearRegression` in scikit-learn; use a residual plot to check model
assumptions.

**Next week:** regularized regression — Ridge, Lasso, and Elastic Net, and why regularization
helps when a model overfits.
