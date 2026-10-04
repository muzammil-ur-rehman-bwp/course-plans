# Lab Notes 8 — Propensity-Score Estimation and IPW-Based ATE Estimation

**Concept recap:** IPW reweights each unit by $1/e(X)$ or $1/(1-e(X))$, correcting for
confounding under unconfoundedness and overlap; the identity
$\mathbb{E}[TY/e(X)-(1-T)Y/(1-e(X))]=\mathrm{ATE}$ is derived via the tower property.

**Common pitfalls:**
- Forgetting to clip $\hat e(X)$ away from 0 and 1 before dividing — even a single estimated
  propensity very close to 0 or 1 can produce an enormous (or undefined) weight that dominates
  the entire estimate; the lecture content's `np.clip(e_hat, 0.02, 0.98)` is not a cosmetic
  detail.
- Fitting the propensity model on $(X,Y)$ instead of $(X,T)$ — the propensity score is
  $\mathbb{P}(T=1\mid X)$, a model of *treatment assignment*, not of the outcome; using $Y$
  anywhere in propensity estimation breaks the entire IPW argument.
- In Task C, reporting only the mean error and overlooking variance — IPW's bias correction can
  come with a real variance cost (as its own derivation's dependence on $e(X)$ in a denominator
  suggests), and a complete comparison should report both.

**Debugging tip:** as a sanity check, set the true propensity function to a constant (e.g.,
`true_propensity = 0.5` for everyone, no actual confounding) and confirm the naive and IPW
estimates nearly agree in this special case — when there is no confounding, IPW should reduce to
(approximately) the naive estimator.

**Instructor tip:** have students derive, on paper, why `e_hat` must be clipped away from exactly
0 or 1 before anyone runs Task D's poor-overlap variant — predicting the failure mode from the
IPW-ATE identity's denominator, before observing it numerically, reinforces the Week 8 overlap
discussion directly.
