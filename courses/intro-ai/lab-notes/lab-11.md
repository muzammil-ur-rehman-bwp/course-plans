# Lab Notes 11 — Bayesian Networks: Inference by Enumeration

**Concept recap:** inference by enumeration computes `P(query | evidence)` by summing the joint
probability (a product of CPT entries, per the network's structure) over all values of the
hidden variables, then normalizing so the result is a valid probability.

**Common pitfalls:**
- Forgetting to normalize — the raw numerator (joint probability of query and evidence) is not
  itself a probability; it must be divided by the sum over all query values.
- Summing over the wrong set of variables — only *hidden* variables (neither the query nor the
  evidence) should be summed out; evidence variables are fixed, not summed.
- Hard-coding a CPT lookup for only one direction (e.g., `P(Alarm=True|...)`) and forgetting the
  complement (`P(Alarm=False|...) = 1 - P(Alarm=True|...)`) is needed too.

**Debugging tip:** compute the *full* joint distribution over all variable combinations and
confirm it sums to exactly 1 before trusting any conditional-probability query built from it —
this single check catches most CPT-encoding bugs immediately.

**Instructor tip:** work the Task D hand-computation on the board *before* lab, live, so
students have a trusted reference answer — debugging an enumeration bug without an independently
verified expected value is frustrating and unproductive.
