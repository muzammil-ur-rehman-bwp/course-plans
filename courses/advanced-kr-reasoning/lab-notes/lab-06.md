# Lab Notes 6 — Distribution Semantics Evaluator

**Concept recap:** each total choice of probabilistic facts induces an ordinary program whose
unique least model is computed by fixpoint iteration; a query's probability sums the total
choices whose induced model entails it — independent-fact mixture, no partition function.

**Common pitfalls:**
- Reimplementing `least_model` with `not` (negation-as-failure) by mistake — this week's rules
  are plain definite clauses with no negation; mixing in ASP-style negation breaks the
  "unique least model" guarantee the distribution semantics relies on.
- Enumerating total choices but forgetting to multiply by the **excluded**-fact probabilities
  `(1 - p)` — a common Task B/C bug computes `prob` using only the included facts' `p` values,
  which does not sum to 1 across all total choices and silently gives a wrong total probability.
- In Task D, writing an MLN encoding that is secretly identical in behavior to the distribution-
  semantics program (e.g., giving every formula an enormous weight to simulate "hard" rules) —
  this defeats the point of the contrast; the written answer should identify a genuine structural
  difference (the need for Z), not paper over it.

**Debugging tip:** for a 2-fact program, manually list all 4 total choices and their probabilities
before running the code; confirm they sum to exactly 1.0 as a basic sanity check on your
probability bookkeeping.

**Instructor tip:** ask students to predict, before coding Task C, how many total choices a
3-probabilistic-fact program has (8) and roughly how the runtime would scale to 20 facts
(2^20 ≈ 1,000,000) — motivates §4's BDD-compilation discussion concretely rather than abstractly.
