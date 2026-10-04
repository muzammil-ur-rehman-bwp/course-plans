# Lab Notes 9 — Brute-Force MLN Evaluator

**Concept recap:** P(x) = (1/Z)exp(Σ w_i n_i(x)), where n_i(x) counts satisfied groundings of
formula i in world x and Z sums the unnormalized score over every possible world.

**Common pitfalls:**
- **Exponential blow-up from too many ground atoms**: `mln_distribution` enumerates 2^|atoms|
  worlds — with 2 constants and a binary predicate like Friends, atom counts grow quickly (6
  atoms already gives 64 worlds); keep Task C's domain exactly at the lecture's scale, not
  larger, or the notebook will stall.
- **Counting groundings of the wrong arity**: `count_satisfied_groundings` infers the number of
  variables from the formula template's argument count — a template written with the wrong
  number of parameters will silently enumerate the wrong number of groundings.
- **Forgetting Z must sum over every world, not just the "plausible" ones**: a shortcut that
  skips low-scoring worlds when computing Z silently produces a non-normalized, incorrect
  distribution — every world must be included in the sum even if its probability ends up
  negligible.
- In Task B, mistaking "very high probability" for "probability exactly 1" — even an extreme
  weight leaves every world with some strictly positive probability under the log-linear model;
  only weight = ∞ (not reachable in finite floating-point arithmetic) is a true hard constraint.

**Debugging tip:** before trusting Task C's two-formula result, re-verify Task A's single-
formula distribution sums to 1.0 (within floating-point tolerance) — a normalization bug is
easiest to catch on the smallest case.

**Instructor tip:** have students compute n_i(x) by hand for at least one world (as in Task D)
before trusting any code output — this is the step most likely to reveal a misunderstanding of
"grounding," which the rest of the week's reasoning depends on.
