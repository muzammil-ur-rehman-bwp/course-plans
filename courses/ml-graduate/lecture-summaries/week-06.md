# Week 6 Summary — Convex Optimization for ML

**Key takeaways:**
- Convex, $L$-smooth objectives converge under gradient descent at rate $O(1/T)$; adding
  $\mu$-strong convexity upgrades this to a linear (geometric) rate governed by the condition
  number $L/\mu$.
- Lagrangian duality turns a constrained problem into an unconstrained dual; weak duality always
  holds, and strong duality holds for convex problems under Slater's condition.
- The KKT conditions (stationarity, feasibility, complementary slackness) are necessary (and, for
  convex problems with strong duality, sufficient) for optimality.
- Applying KKT to the SVM primal recovers its dual, expressed purely in inner products — exactly
  what makes the kernel trick possible.

**You should now be able to:** derive both GD convergence rates and derive the SVM dual from its
primal via the KKT conditions.

**Next week:** kernel methods and RKHS theory — making the kernel trick rigorous via the
representer theorem.
