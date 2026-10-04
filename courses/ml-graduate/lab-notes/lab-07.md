# Lab Notes 7 — Kernel Ridge Regression via the Representer Theorem

**Concept recap:** the representer theorem guarantees the RKHS-optimal solution is always a
finite kernel expansion $\hat f(\cdot)=\sum_i\alpha_ik(x_i,\cdot)$; kernel ridge regression solves
for $\alpha$ via $(K+\lambda mI)\alpha=y$, matching `sklearn.kernel_ridge.KernelRidge`'s internal
convention once $\lambda$ is scaled consistently.

**Common pitfalls:**
- **Forgetting that a valid kernel must be positive semi-definite** — if a custom kernel is used
  instead of RBF, always check its Gram matrix's eigenvalues before trusting any downstream
  result; a kernel that is not PSD breaks the entire RKHS construction (there is no valid Hilbert
  space / reproducing property to appeal to) and the representer-theorem-based solve can silently
  return a nonsensical or unstable result.
- Mismatching scikit-learn's `alpha` parameter (which plays the role of $\lambda\cdot m$, not
  $\lambda$ alone) against the from-scratch $\lambda$, producing an apparent "mismatch" that is
  actually just a unit-convention difference — always re-derive the exact correspondence rather
  than assuming `alpha=lambda`.
- Choosing a kernel bandwidth (`gamma`) so large that the Gram matrix is nearly the identity
  matrix (every off-diagonal entry near 0), which makes every fit look deceptively good on
  training data while generalizing poorly — a kernel-method analogue of Week 1's overfitting
  lesson.

**Debugging tip:** if Task C's max absolute difference is not near machine precision, print both
$\lambda$ values being used (from-scratch vs. `KernelRidge.alpha`) side by side — a unit-convention
mismatch is the most common cause, not a logic error in the solve itself.

**Instructor tip:** use Task D's three-curve comparison to connect this lab visually back to
Week 9's upcoming ridge-regression-as-MAP lecture — the same under/over-regularization tradeoff
will reappear there in a probabilistic form.
