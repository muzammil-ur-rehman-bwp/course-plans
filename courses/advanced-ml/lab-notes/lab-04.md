# Lab Notes 4 — Simulating the Marchenko–Pastur Law

**Concept recap:** the Marchenko–Pastur law gives the limiting spectral distribution of a sample
covariance matrix with aspect ratio $\gamma=p/n$, with support
$[(1-\sqrt\gamma)^2,(1+\sqrt\gamma)^2]$ — even when the true covariance is the identity.

**Common pitfalls:**
- Normalizing the sample covariance incorrectly — the lecture-content formula is
  $\hat\Sigma=\frac1n XX^\top$ for a $p\times n$ matrix $X$; dividing by $p$ instead of $n`, or
  using an $n\times p$ matrix without transposing, silently shifts the empirical spectrum away
  from the predicted density.
- In Task B, using a strict inequality where the predicted edge itself should be included (a
  small number of eigenvalues landing very close to, but technically just outside, $[a,b]$ is
  expected finite-size fluctuation, not necessarily a bug) — report the fraction with a sensible
  tolerance rather than treating any edge-crossing as an error.
- In Task D, expecting the smallest eigenvalue to hit exactly 0 at $\gamma=0.95$ or $\gamma=0.99$
  — the law only guarantees this in the $p,n\to\infty$ limit; at finite $p,n$ expect it to be
  small and visibly shrinking as $\gamma\to1$, not exactly zero.

**Debugging tip:** first test `mp_density` at $\gamma=0.0001$ (nearly classical regime) and
confirm the support interval collapses to nearly $\{1\}$, matching the lecture content's
boundary-case discussion, before trusting the function at the harder $\gamma$ values.

**Instructor tip:** ask students, before running Task D, to predict whether the standard
deviation of the largest eigenvalue (Task C) will grow or shrink as $\gamma\to1$ — most correctly
guess "grow," but few can explain why without re-deriving that the spectral edge itself becomes
less sharply defined at finite $n$ as $\gamma$ approaches 1, a good prompt for discussion.
