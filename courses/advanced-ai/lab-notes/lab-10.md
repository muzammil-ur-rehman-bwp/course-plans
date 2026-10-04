# Lab Notes 10 — Permutation Importance and a Sanity-Check Failure

**Concept recap:** permutation importance measures performance degradation when a feature's
values are shuffled, requiring no differentiability; the randomization sanity check compares an
attribution method's output on the real trained model vs. a version with randomized parameters —
a method insensitive to this randomization cannot be reflecting what the model actually learned.

**Common pitfalls:**
- Forgetting to copy `X` before shuffling a column in-place (`X_perm = X.copy()`), which can
  silently mutate the shared dataset across repeats and corrupt later cells in the notebook — the
  Week 10 lecture content's implementation does this deliberately; do not remove it.
- Building the synthetic dataset's true rule so weakly (e.g., too little signal relative to
  noise) that even features 0 and 1 barely beat the noise features in Task B — make the
  true-feature signal strong enough that the ranking is unambiguous, since the point of the task
  is to validate the *method*, not to stress-test it on a hard case.
- In Task C, implementing the "naive" correlation method correctly but then being surprised it
  "fails" the sanity check — this is the expected, intended result: a method that never looks at
  the model's parameters at all cannot possibly be sensitive to randomizing them, which is
  exactly the lecture's point, not a bug to fix.
- Confusing "this method passed the sanity check" with "this method is proven correct" in the
  Task D discussion — passing is necessary, not sufficient, evidence of genuine mechanism (Week
  10, §3); do not overstate the conclusion.

**Debugging tip:** verify `permutation_importance`'s `baseline_loss` is computed once outside the
per-feature loop, not recomputed inconsistently inside it — recomputing it per feature can
silently change what "increase in loss" means between features if any randomness enters the
baseline prediction itself.

**Instructor tip:** before running Task D, ask students to predict whether permutation
importance will look like real signal or like noise under model-parameter randomization — most
students correctly predict it should look like noise (since it does query the actual model's
predictions), which is a useful contrast to set up against Task C's naive method, which fails for
a structurally different reason (it never queries the model at all).
