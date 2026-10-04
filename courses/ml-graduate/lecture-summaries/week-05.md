# Week 5 Summary — Concentration Inequalities

**Key takeaways:**
- Markov's and Chebyshev's inequalities give polynomial tail decay; Hoeffding's inequality
  improves this to exponential decay for bounded, independent variables, via Hoeffding's lemma
  (an MGF bound) and Chernoff bounding.
- Hoeffding's inequality: $\Pr[|\widehat\mu-\mu|\geq\epsilon]\leq2\exp(-2m\epsilon^2)$ for
  i.i.d. $[0,1]$-bounded variables — the exact tool Weeks 2–4 used as a black box.
- McDiarmid's inequality generalizes this to any bounded-differences function of independent
  variables, which is exactly what is needed to bound $\sup_h(L_D(h)-\widehat L_S(h))$ as a
  function of the whole sample.
- Both inequalities require independence and boundedness; applying them to correlated or
  unbounded data invalidates the stated guarantee.

**You should now be able to:** prove Hoeffding's inequality in full and identify when its
assumptions do and do not hold.

**Next week:** convex optimization for ML — gradient descent convergence rates, Lagrangian
duality, and the KKT conditions.
