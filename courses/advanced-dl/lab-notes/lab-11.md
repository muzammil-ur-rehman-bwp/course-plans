# Lab Notes 11 — Compute-Optimal Allocation and Checkpoint Intervals

**Concept recap:** compute-optimal allocation grows $N$ and $D$ together at fitted, generally
unequal exponents under $C\approx 6ND$; the optimal checkpoint interval
$\tau^*=\sqrt{2c/\lambda}$ balances checkpointing overhead against expected lost compute on
failure.

**Common pitfalls:**
- Choosing exponents $a,b$ in Task A that do not approximately satisfy $a+b\approx 1$ — this
  breaks the consistency with $C\approx 6ND$ the lecture derivation relies on; sanity-check your
  chosen exponents before running Task B.
- In Task B, plotting loss on a linear rather than log scale for $C$ — scaling-law-style
  relationships are far more legible on a log-log or semi-log plot, and a linear plot can make a
  real, meaningful gap look negligible.
- In Task C, computing $\tau^*$ correctly but then failing to confirm it numerically against
  `expected_lost_compute`'s minimum — always plot the function and verify the closed-form answer
  actually sits at the visible minimum, rather than trusting the formula alone.
- In Task D, drifting into explaining *why* the scaling exponents take the values they do (the
  sibling course's question) rather than stating the engineering allocation question this course
  asks — re-read the Week 11 lecture content's Section 1 scoping statement if this happens.

**Debugging tip:** verify `expected_lost_compute` is convex in $\tau$ over a reasonable range by
plotting it before computing $\tau^*$ analytically — if the plotted curve has no clear single
minimum, double-check the sign and scale of your chosen `checkpoint_cost` and `failure_rate`.

**Instructor tip:** ask students to predict, before running Task B, whether the naive allocation's
disadvantage grows, shrinks, or stays constant (in absolute loss terms) as $C$ increases — most
scaling-law-consistent setups show a growing absolute gap, a useful concrete illustration of why
compute-optimal allocation matters more, not less, as training budgets scale up.
