# Week 5 Summary — Full-Information Online Convex Optimization

**Key takeaways:**
- The OCO protocol reveals the full convex loss function $f_t$ each round — strictly more
  information than the sibling course's bandit setting, though still with no statistical
  assumption on the loss sequence.
- FTRL stabilizes "follow the leader" via a strongly convex regularizer; online gradient descent
  is FTRL applied to a linearized loss, i.e. projected gradient descent.
- OGD's regret bound $\mathrm{Regret}_T \leq DG\sqrt T$ is derived via projection
  non-expansiveness and a telescoping sum, with $\eta=D/(G\sqrt T)$ (or $\eta_t=D/(G\sqrt t)$)
  optimizing the bound.

**You should now be able to:** state the OCO protocol and regret, and explain precisely how it
differs from the bandit setting; derive OGD's $O(\sqrt T)$ regret bound; implement FTRL and OGD
and compare their empirical regret.

**Next week:** Nonparametric Bayesian methods I — the Dirichlet process and its stick-breaking
construction.
