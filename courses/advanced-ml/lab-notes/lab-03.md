# Lab Notes 3 — Simulating Sub-Gaussian and Sub-Exponential Concentration

**Concept recap:** sub-Gaussian variables have MGF bounds holding for all $\lambda$, giving
Gaussian-type tails everywhere; sub-exponential variables only have this bound near $\lambda=0$,
so Bernstein's inequality is needed, with a crossover from Gaussian-like to purely exponential
decay at $t\approx n\nu^2/\alpha$.

**Common pitfalls:**
- Using the *unnormalized sum* instead of the *mean* in the empirical-tail computation — the
  lecture-content bounds are stated for $S_n=\sum_i X_i$ (or implicitly for the mean via rescaling
  $t$); check carefully which quantity (`mean(row)` vs. `sum(row)`) each formula in Task B expects,
  and keep it consistent between your simulation and the bound you compare it to.
- In Task C, concluding the sub-Gaussian bound "always fails" for the chi-square case — it is
  actually a *valid*, if often loose, bound at small $t$ (both tails decay similarly there); the
  failure is specifically a large-$t$ phenomenon, exactly where the true tail is heavier than
  Gaussian.
- Off-by-one errors in the two-regime Bernstein formula: confirm you are taking the `min` of the
  two candidate exponents, not accidentally always using one regime's formula.

**Debugging tip:** print both candidate exponents ($t^2/(n\nu^2)$ and $t/(n\alpha)$, inside the
`min`) separately at each $t$ in Task B, and confirm which regime is active at each value — this
makes the crossover point in Task D directly visible in your own printed output before you need
to argue about it.

**Instructor tip:** have students compute, by hand, the chi-square-1 sub-exponential parameters
$(\nu^2,\alpha)=(2,4)$ from the MGF formula $\mathbb{E}[e^{\lambda X}]=e^{-\lambda}/\sqrt{1-2\lambda}$
given in lecture, rather than simply accepting the stated values — this reinforces that Bernstein's
parameters are not arbitrary tuning constants but are derived from the variable's actual MGF.
