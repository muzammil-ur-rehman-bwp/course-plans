# Lab Notes 14 — A Toy Binned Mutual-Information Estimate

**Concept recap:** this lab's mutual-information estimates are a deliberately crude, binned
approximation for a continuous, high-dimensional quantity that is genuinely hard to estimate
correctly — the lab's instructional goal is to let students feel *why* the published critiques of
information-bottleneck claims in deep learning center on estimator sensitivity, not to produce a
publication-quality MI estimate.

**Common pitfalls:**
- **Treating the binned MI numbers as precise, trustworthy measurements** rather than a
  qualitative, estimator-dependent sketch — Task C exists specifically to prevent this by making
  the numbers visibly move under an essentially arbitrary choice (`n_bins`).
- Using a fixed bin *count* without fixing bin *edges* consistently across epochs — if bin edges
  are recomputed from each epoch's activation range separately, changes in the MI curve can
  reflect changing activation scale/range over training, not changing information content, which
  is exactly the kind of estimator artifact the published critiques raise.
- In Task B, over-interpreting a different-looking curve for tanh vs. ReLU as proof of one
  specific causal story, rather than reporting it as consistent (or not) with the published
  activation-function-dependence observation — this lab is not designed to *settle* the debate,
  only to let students observe the kind of empirical sensitivity that fuels it.
- Forgetting that mutual information is always $\geq 0$; a negative value from a buggy
  implementation (e.g., a sign error or division-by-zero guard gone wrong) is a sure sign of a
  bug, not a valid (if unusual) result.

**Debugging tip:** if MI estimates come out negative or wildly unstable, check the handling of
empty bins (`p_b == 0` or `p_bc == 0` cases) in the estimator — these must be skipped (contribute
$0$), not produce a `log(0)` or divide-by-zero.

**Instructor tip:** use this lab's Task C explicitly to connect back to Week 14's "how to hold a
debated idea honestly" framing (Section 3 of the lecture content) — the estimator sensitivity
students observe firsthand here is the concrete version of the abstract methodological point.
