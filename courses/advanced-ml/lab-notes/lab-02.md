# Lab Notes 2 — Empirical Fano-Bound Sanity Check

**Concept recap:** Fano's inequality forces a testing-error probability bound that translates,
via a packing-set argument, into a minimax estimation lower bound; for the Gaussian location
family at the critical 2-point separation $\epsilon^\star=\sqrt{\ln2/(4n)}$, testing should still
be genuinely hard, while at a larger separation it becomes easy.

**Common pitfalls:**
- Forgetting that `two_point_test_error` uses the Bayes-optimal decision rule (sign of the sample
  mean) — a different, suboptimal rule would not test the sharpness of the critical scale
  correctly, since Fano bounds the *best possible* estimator's error, not any one specific rule's.
- Misreading Task C's log-log slope: a slope of $-1/2$ confirms $\epsilon^\star \propto
  n^{-1/2}$; a common error is plotting $n$ on a linear axis against $\epsilon^\star$ and
  concluding (incorrectly) that the relationship looks "roughly linear" at small $n$, when it is
  genuinely a power law only visible correctly on log-log axes.
- In Task D, using a decision rule that does not generalize correctly to 4 points (e.g., reusing
  the 2-point sign-based rule unchanged) — the correct generalization is "predict whichever of
  the 4 candidate means is closest to the observed sample mean."

**Debugging tip:** before trusting Task B's numbers, sanity-check `two_point_test_error` at a very
large $\epsilon$ (e.g., $\epsilon=5$) and confirm the empirical error is close to 0 — a basic
correctness check independent of the critical-scale question.

**Instructor tip:** ask students to predict, before running Task D, whether the 4-point critical
constant will be larger or smaller than the 2-point one — most predict the wrong direction at
first, which is a productive point to resolve by directly inspecting the KL-divergence bound's
dependence on $M$.
