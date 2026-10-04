# Lab Notes 10 — Gaussian Process Regression From Scratch

**Concept recap:** GP regression's predictive mean and covariance follow from exact Gaussian
conditioning; the kernel's length scale controls smoothness, and the noise variance controls how
tightly the mean is pulled through observed points; Cholesky decomposition is used for numerical
stability, not just speed.

**Common pitfalls:**
- **Numerically unstable Cholesky decomposition in a from-scratch GP implementation** — this is
  one of the most common real-world GP bugs. A kernel matrix with closely-spaced points and a
  short length scale can become so ill-conditioned that `cho_factor` raises a
  `LinAlgError` ("matrix is not positive definite") due purely to floating-point round-off, even
  though the true kernel matrix is mathematically PSD. The standard fix is adding a small "jitter"
  term (e.g., `1e-6 * np.eye(m)`) to the diagonal before factorizing.
- **Forgetting that the kernel must be positive semi-definite** in the first place — an
  incorrectly implemented custom kernel (e.g., one with a sign error) can produce a matrix that
  fails Cholesky factorization for a genuine mathematical reason, not merely a numerical one;
  always distinguish the two causes before adding jitter as a blanket fix.
- Confusing the *prior* covariance (before seeing any data, $K_{\star\star}$ alone) with the
  *posterior* covariance (after conditioning, Section 3's full formula) when discussing why
  uncertainty shrinks near training data.

**Debugging tip:** if Task D's direct-inversion comparison raises a `LinAlgError` or produces
wildly different results from the Cholesky version, that is the expected, intended demonstration
of numerical instability — do not "fix" it by abandoning the comparison; report and explain the
discrepancy instead.

**Instructor tip:** show students the exact `LinAlgError` traceback once, live, from a
deliberately too-short length scale — recognizing this specific failure mode by sight will save
them significant debugging time in the capstone if their chosen topic involves GPs.
