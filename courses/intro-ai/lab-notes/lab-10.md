# Lab Notes 10 — Uncertainty: Bayes' Rule for Diagnostic Testing

**Concept recap:** Bayes' rule converts a prior, a likelihood (sensitivity/false-positive rate),
and a normalizing constant (total probability of the evidence) into a posterior; the posterior
is highly sensitive to the prior when the condition is rare, which is the source of the classic
"positive test, still probably healthy" result.

**Common pitfalls:**
- Confusing sensitivity (`P(+|D)`) with the posterior (`P(D|+)`) — these are not the same
  quantity, and conflating them is exactly the base-rate-neglect error the lecture warns about.
- Computing `P(+)` incorrectly by forgetting the `P(+|¬D) * P(¬D)` term — without the law of
  total probability, the denominator is wrong and the posterior will not be a valid probability.
- Chaining two tests (Task D) by re-using the *original* prior instead of the *updated*
  posterior from the first test as the new prior — this is the step students most often get
  backwards.

**Debugging tip:** always sanity-check that `P(D|+) + P(¬D|+) == 1` (compute both and sum) —
if the sum is not 1, there is a normalization bug in the denominator calculation.

**Instructor tip:** before revealing the "surprising" low-posterior result, ask students to
*guess* the answer first — the gap between intuition and the computed result is the single most
memorable moment in this unit, and it is wasted if the code just confirms what students already
expected.
