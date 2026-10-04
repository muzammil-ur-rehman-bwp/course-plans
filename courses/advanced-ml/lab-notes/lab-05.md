# Lab Notes 5 — FTRL and Online Gradient Descent

**Concept recap:** OGD is FTRL applied to a linearized loss; its regret bound $DG\sqrt T$ comes
from projection non-expansiveness plus a telescoping sum, optimized by $\eta=D/(G\sqrt T)$ (or
$\eta_t=D/(G\sqrt t)$ if $T$ is unknown in advance).

**Common pitfalls:**
- Forgetting to project onto the feasible set after the gradient step — without
  `project_to_ball`, $x_t$ can leave $\mathcal K$ entirely, invalidating both the algorithm and
  the regret-bound derivation, which assumes $x_t\in\mathcal K$ throughout.
- Using a constant step size tuned for one specific $T$ and then comparing regret across
  different horizons — the bound $DG\sqrt T$ assumes $\eta=D/(G\sqrt T)$ is tuned to the specific
  $T$ being run; a step size tuned for $T=500$ is not optimal, and should not be expected to be
  optimal, at $T=5000$.
- In Task D, applying the quadratic-loss closed-form FTRL update unchanged to the absolute-value
  loss sequence — this closed form was derived specifically for $f_t(x)=\tfrac12\|x-a_t\|^2$ and
  does not apply to a non-quadratic loss; a general FTRL implementation would need to solve the
  regularized minimization numerically at each round instead.

**Debugging tip:** verify your OGD implementation on a trivial 1-round case where the hindsight
optimum is obviously $x^\star = a_1$ and confirm regret is near zero, before trusting the full
$T=500$ experiment.

**Instructor tip:** have students predict, before running Task B, whether FTRL or OGD will have
lower regret on the quadratic loss sequence — since FTRL's closed form is the exact regularized
running-average minimizer for quadratic losses specifically, it typically edges out OGD slightly
here, a good concrete illustration of why the "right" algorithm can depend on loss structure.
