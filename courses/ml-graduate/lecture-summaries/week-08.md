# Week 8 Summary — Rigorous Ensemble Theory; Midterm Review

**Key takeaways:**
- AdaBoost's training error is bounded by $\prod_t2\sqrt{\epsilon_t(1-\epsilon_t)}$, which
  decays exponentially fast to zero whenever each weak learner beats chance by a fixed margin.
- Margin theory explains why boosting often keeps improving generalization *after* training
  error reaches zero: the margin-based generalization bound has no explicit dependence on the
  number of rounds $T$, and empirical margins keep growing even once training error is flat.
- Bagging's variance formula, $\mathrm{Var}(\bar h)=\sigma^2/n+\frac{n-1}{n}\rho\sigma^2$, shows
  averaging shrinks the independent-noise term but leaves a correlation-driven variance floor —
  exactly why Random Forests add random feature subsets to further decorrelate trees.
- Midterm (next week) covers all of Weeks 1–8.

**You should now be able to:** derive AdaBoost's training-error bound and bagging's variance
formula, and self-assess readiness for the midterm.

**Reminder:** Assignment 2 is due at the start of this week. Midterm Exam is next week (Week 9).
