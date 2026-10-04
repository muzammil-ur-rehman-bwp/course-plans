# Week 8 Summary — Causal Inference in Depth I: Potential Outcomes and Propensity Scores

**Key takeaways:**
- The potential-outcomes framework defines $Y_i(1),Y_i(0)$ per unit; only one is ever observed
  (the fundamental problem of causal inference), so the ATE, not the individual effect, is the
  estimable quantity.
- Confounding is formalized as $T \not\perp (Y(0),Y(1))$; unconfoundedness ($T\perp(Y(0),Y(1))\mid
  X$) and overlap ($0<e(X)<1$) are the two assumptions licensing causal estimation from
  observational data.
- The IPW-ATE identity, $\mathbb{E}[TY/e(X) - (1-T)Y/(1-e(X))] = \mathrm{ATE}$, is derived via the
  tower property and unconfoundedness, and recovers the true ATE on simulated confounded data
  where the naive difference-in-means estimator is biased.

**You should now be able to:** state the potential-outcomes framework and the fundamental problem
of causal inference; state unconfoundedness and overlap precisely; derive and implement the
IPW-ATE estimator.

**Midterm (Week 9):** covers Weeks 1–8, qualifying-exam style.

**Next week:** Midterm Exam, then Causal inference in depth II — instrumental variables and the
do-calculus rules.
