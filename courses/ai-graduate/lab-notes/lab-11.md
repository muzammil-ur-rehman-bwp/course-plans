# Lab Notes 11 — Approximate Inference: Rejection Sampling & Likelihood Weighting

**Concept recap:** rejection sampling discards samples inconsistent with evidence (wasteful when
evidence is rare); likelihood weighting fixes evidence variables and weights each sample by
evidence likelihood, using every sample.

**Common pitfalls:**
- Sampling evidence variables in likelihood weighting instead of *fixing* them to their observed
  values — this is the single defining difference from rejection sampling; accidentally sampling
  them (then multiplying by their CPT probability as if that were a weight) produces a
  subtly-wrong estimator that is neither correct likelihood weighting nor correct rejection
  sampling.
- Forgetting to normalize likelihood weighting's final estimate by the *total weight* (not the
  sample count) — the estimator is Σ(weight where query=True) / Σ(all weights), not a plain
  average over sample count.
- In Task B, raising/ignoring the zero-samples-kept edge case silently (e.g., returning NaN or
  an arbitrary default) instead of recognizing that extremely rare evidence can make rejection
  sampling fail outright within a fixed sample budget — this failure mode is itself the point
  being demonstrated, not a bug to suppress.
- In Task D, comparing only the two methods' *mean* estimates and ignoring the spread — the
  lab's actual pedagogical point is the standard deviation comparison; a report with only means
  has missed the exercise's purpose.

**Debugging tip:** verify your Bayesian network's CPTs sum to 1 for every parent-value
combination before running any inference — if `cpt[var][parent_vals]` represents P(var=True),
double-check you have not accidentally entered P(var=False) in some entries.

**Instructor tip:** have students predict, before running Task D, which method they expect to
have lower variance at rare evidence, then check their prediction against the actual
standard-deviation numbers — this reinforces the Week 11 argument about likelihood weighting's
efficiency advantage as something to be verified, not taken on faith.
