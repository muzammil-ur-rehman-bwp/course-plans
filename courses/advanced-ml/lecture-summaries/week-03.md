# Week 3 Summary — High-Dimensional Statistics I: Concentration Beyond Hoeffding

**Key takeaways:**
- Sub-Gaussian variables ($\mathbb{E}[e^{\lambda X}]\leq e^{\lambda^2\sigma^2/2}$ for all
  $\lambda$) generalize bounded variables (Hoeffding's lemma) and Gaussians, giving Gaussian-type
  tail decay $e^{-t^2/2\sigma^2}$.
- Sub-exponential variables relax the MGF bound to hold only for $|\lambda|\leq 1/\alpha$,
  correctly modeling heavier-tailed quantities like centered $\chi^2$ variables.
- Bernstein's inequality has two regimes — sub-Gaussian-like near the mean, purely exponential in
  the tail — with the transition at $t \approx n\nu^2/\alpha$.

**You should now be able to:** define sub-Gaussian and sub-exponential variables; derive
Bernstein's inequality via the Chernoff/MGF argument; simulate and empirically verify both tail
bounds, including confirming a sub-Gaussian bound fails where Bernstein's does not.

**Next week:** High-dimensional statistics II — random matrix theory basics and the
Marchenko–Pastur law.
