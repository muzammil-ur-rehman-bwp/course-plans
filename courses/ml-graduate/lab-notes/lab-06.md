# Lab Notes 6 — Gradient Descent Convergence Rates

**Concept recap:** convex + smooth objectives converge at rate $O(1/T)$ under gradient descent;
adding strong convexity upgrades this to a linear (geometric) rate; the step size $\eta=1/L$ used
throughout is what the derivation assumes — a different step size invalidates the stated rate.

**Common pitfalls:**
- Using a step size larger than $1/L$ (where $L$ is the true Lipschitz constant of the gradient,
  i.e., the largest eigenvalue of $A$ for a quadratic) — this can make gradient descent diverge or
  oscillate, which is not a bug in the convergence-rate theory but a violation of its step-size
  precondition.
- Plotting $f(x_t)-f^\star$ on a linear scale for the convex-only case, where the $O(1/T)$ decay
  is easy to mistake visually for "not converging" — use a log-log scale as instructed.
- Forgetting that a singular (rank-deficient) $A$ is merely convex, not strongly convex (since its
  smallest eigenvalue is $0=\mu$) — Task B requires this distinction to be deliberate, not
  accidental.

**Debugging tip:** if the strongly-convex run's log-linear plot isn't a straight line, print the
computed condition number $L/\mu$ and confirm $\eta=1/L$ was actually used (not, e.g., a fixed
`eta=0.01` left over from testing).

**Instructor tip:** have students compute the theoretical iteration count to reach a fixed
tolerance from the derived rate formula, then compare it to the empirical iteration count from
Task D — the two should match in order of magnitude, reinforcing that the derived bound is not
just qualitatively but quantitatively predictive.
