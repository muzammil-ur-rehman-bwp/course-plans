# Lab Notes 9 — VC Dimension and the Empirical/True Risk Gap

**Concept recap:** VC dimension is defined via **shattering** — the largest $m$ for which *some*
set of $m$ points can realize *every* possible labeling — and must be computed by exhibiting (for
the lower bound) a shattered set and (for the upper bound) proving no larger set can be shattered.
It is a property of the hypothesis class as a whole, not of any particular model's parameter
count.

**Common pitfalls:**
- **The single most common conceptual error in this lab: confusing VC dimension with number of
  parameters.** Task D is designed specifically to break this habit — a 1-parameter threshold
  class already has infinite-ly many possible thresholds but VC dimension exactly 1 (it can
  shatter 1 point but not 2, since no threshold can label two points as (1,0) *and* separately
  realize (0,1) with one threshold's monotone structure — check this directly), while a
  50-interval-union class with far more "parameters" has a much larger VC dimension — but neither
  number is literally the raw parameter count, and for some classes VC dimension can even be
  **infinite** despite a small number of real-valued parameters (e.g., $\sin(\omega x)$
  thresholded, varying $\omega$) — a fact worth mentioning but not required to prove here.
- Proving a class shatters $m$ points by checking only a few of the $2^m$ labelings rather than
  all of them — a correct shattering proof (Tasks A/B) must address every labeling, either by
  full enumeration (small $m$) or by a general argument.
- Concluding a class's VC dimension is $m$ just because *some* set of $m$ points is shattered,
  without separately checking that *no* set of $m+1$ points can be — both directions are required
  by the definition.
- In Task C, fitting the $O(1/\sqrt m)$ shape with a sign or constant error and concluding the
  bound "doesn't fit" — the bound is an upper bound with an unspecified constant, not an exact
  equality; the qualitative shape (not exact match) is what should be checked.

**Debugging tip:** if Task B's 4-point shattering counterexample is hard to find, try 4 points at
the corners of a square — labeling diagonal pairs identically and adjacent pairs oppositely is a
good general-purpose example that fails for linear separators.

**Instructor tip:** ask every student, out loud, to state the VC dimension vs. parameter-count
distinction in their own words before moving to Task C — this is the single concept this lab most
needs to land correctly before Week 10's Rademacher-complexity material builds on it.
