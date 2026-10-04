# Lab Notes 10 — Density-Ratio Estimation and the Overlap Failure Mode

**Concept recap:** importance weighting corrects for covariate shift by reweighting training
points by $w(x)=P_{\mathrm{te}}(x)/P_{\mathrm{tr}}(x)$, estimated via a domain classifier's odds;
poor overlap drives $w(x)$'s variance up, degrading the correction's practical value.

**Common pitfalls:**
- Forgetting to clip the domain classifier's predicted probability away from 0 and 1 before
  computing odds — exactly the same failure mode as Week 8's propensity-score clipping, now in a
  different (domain-classification) guise.
- Omitting the base-rate rescaling term $P(\mathrm{domain}=0)/P(\mathrm{domain}=1)$ when the
  training and test sample sizes used to fit the domain classifier are unequal — in the lecture
  content's code this term is 1 because `n` is equal for both, but in Task C/D with different
  sample sizes it must be included explicitly.
- In Task D, expecting importance weighting to *always* help — at `shift=5.0`'s poor overlap,
  the weighted model's high-variance weights can sometimes make it perform comparably to, or even
  worse than, the unweighted model on a single draw; report this honestly rather than forcing the
  conclusion that weighting always wins, and discuss it as a direct illustration of Week 10's
  "structural limit" point.

**Debugging tip:** before trusting Task C/D's regression comparison, print the five largest
weights at each shift level — if a tiny handful of training points carry almost all the total
weight, that alone explains any instability observed downstream.

**Instructor tip:** have students predict, before running Task D, whether weighting will help
more at `shift=0.5` or `shift=5.0` — most correctly guess "more at low shift, where overlap is
good and weights are stable," which is the opposite of where the *correction* is most needed; this
tension (most needed where least reliable) is the core lesson of the overlap failure mode.
