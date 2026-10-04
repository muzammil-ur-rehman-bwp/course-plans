# Lab Notes 9 — Support Vector Machines

**Concept recap:** SVMs maximize the margin between classes; `C` controls tolerance for margin
violations, and the kernel (linear/polynomial/RBF) controls the shape of the decision boundary.

**Common pitfalls:**
- Forgetting to scale features before fitting an SVM — margins and kernel values depend directly
  on distances, so unscaled features badly distort the fitted boundary.
- Choosing an RBF kernel by default without checking whether a linear kernel already performs
  well — an unnecessarily non-linear kernel adds variance risk and reduces interpretability for
  no benefit on data that is actually close to linearly separable.
- Setting `gamma` far too large on the RBF kernel, producing an overfit, highly irregular
  boundary that wraps tightly around individual training points.

**Debugging tip:** if the RBF-kernel SVM achieves ~100% training accuracy but poor test accuracy,
try lowering `gamma` and/or `C` — both are classic overfitting knobs for this model.

**Instructor tip:** have students fit a linear-kernel SVM first on every dataset as a baseline
before trying RBF/polynomial kernels, to build the habit of checking whether the extra complexity
is actually earning its keep.
