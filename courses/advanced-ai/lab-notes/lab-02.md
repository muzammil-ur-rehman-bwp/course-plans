# Lab Notes 2 — Multiplicative Weights

**Concept recap:** multiplicative weights multiplicatively shrinks each expert's weight by
(1 − η·loss); its regret against any fixed expert is bounded by ηT + (ln N)/η, derived via a
potential-function argument.

**Common pitfalls:**
- Choosing η > 1/2 breaks the ln(1 − x) ≥ −x − x² inequality the derivation relies on, and can
  even drive a weight negative if a loss is at its upper bound — always keep η ≤ 1/2 in practice,
  and well below it when T is large (the optimal η = √(ln N / T) shrinks as T grows).
- Confusing "regret" with "loss" when plotting — regret is the *difference* between the
  algorithm's cumulative loss and the best fixed expert's cumulative loss, not either quantity
  alone; a common Task B bug is plotting raw losses and mistaking a non-flat curve for "high
  regret" when the gap between the two curves is actually small and shrinking relative to T.
- In Task D, using too few η values or too narrow a range to see the predicted U-shape; include
  at least one η an order of magnitude away from the theoretical optimum on each side.

**Debugging tip:** verify your implementation on a trivial 2-round, 2-expert case where you can
compute both experts' weights by hand before trusting it on the full T=2000 experiment.

**Instructor tip:** have students predict, before running Task D, whether a too-small or
too-large η will hurt regret more in the T=2000 regime — this reinforces reading the bound
formula as a predictive tool, not just a post-hoc description, mirroring the graduate course's
own instructor-tip pattern for complexity analysis.
