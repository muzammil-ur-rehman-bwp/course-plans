# Week 13 Summary — Causal Inference Basics

**Key takeaways:**
- Correlation and causation can diverge sharply; a confounder is a common cause that creates
  spurious association between two otherwise unrelated (or oppositely related) variables.
- Simpson's paradox — a worked numerical treatment table — shows an aggregate trend can reverse
  every one of its own subgroup trends once a confounder (severity) is stratified on.
- Causal DAGs distinguish confounders (should be adjusted for), mediators (should not), and
  colliders (conditioning on them can create spurious association) — the same adjustment is
  correct for one and wrong for the others.
- Do-notation, $P(Y\mid\mathrm{do}(X=x))$ vs. $P(Y\mid X=x)$, conceptually distinguishes
  intervention from observation — directly relevant to trustworthy, decision-driving ML.

**You should now be able to:** construct and explain a Simpson's-paradox example and classify a
variable's causal role in a simple scenario.

**Next week:** model selection theory — the bias-variance decomposition, AIC/BIC, and
cross-validation's theoretical justification.
