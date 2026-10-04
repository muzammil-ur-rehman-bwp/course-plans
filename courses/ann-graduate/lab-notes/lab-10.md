# Lab Notes 10 — Rademacher Complexity and Random-Label Fitting

**Concept recap:** empirical Rademacher complexity is computed by averaging, over many random
$\pm1$ label draws, the *best possible* correlation some hypothesis in the class achieves with
that pure noise — it is a measure of worst-case-over-random-noise fit quality on a *specific*
sample, not a property of a single trained model.

**Common pitfalls:**
- Computing the Rademacher estimate using only **one** draw of $\sigma$ instead of averaging over
  many (the definition is an *expectation* over $\sigma$) — a single draw is a noisy, unreliable
  estimate and can mislead the Task A trend.
- **Misinterpreting the random-label result as "the network memorized, so it generalizes
  worse."** The point of Task B is subtler: the *same* network, the *same* training procedure,
  reaches near-zero training error in *both* conditions, yet only the real-labels run generalizes
  — the capacity to fit noise does not, by itself, explain or predict what happens on real data;
  conflating "can memorize" with "will overfit on real data" is exactly the mistake Week 10's
  lecture warns against.
- In Task C, running too few training epochs to reach convergence and concluding the network
  "can't fit" a given random-label sample size, when more training steps would have succeeded —
  distinguishing a true capacity limit from an under-training artifact requires checking the
  training loss curve has actually plateaued.
- In Task D, computing margin with the wrong sign convention (not accounting for which class the
  point actually belongs to), which can make a clearly well-separated point register as having a
  "negative" margin.

**Debugging tip:** if Task B's random-label run does not reach near-100% training accuracy, check
network capacity (hidden width) and training epochs/learning rate before concluding anything
about the underlying theory — the phenomenon requires an actually-overparameterized-enough setup.

**Instructor tip:** have students state, explicitly and in writing, the one-sentence distinction
between "raw capacity" and "what training actually finds" using their own Task B/D numbers as
evidence — this is the single idea Weeks 9–11 are building toward.
