# Lab Notes 15 — Pipelines, Persistence & Fairness Audit

**Concept recap:** a full `Pipeline` bundles preprocessing and modeling into one deployable,
leak-safe object; fairness auditing means checking performance separately across groups, not
only in aggregate.

**Common pitfalls:**
- Saving only the trained model (not the full pipeline including preprocessing) with `joblib`,
  which breaks at inference time if raw, unprocessed data is fed to it.
- Computing only overall accuracy/recall and skipping the per-group breakdown — a model can look
  excellent in aggregate while performing poorly for a minority group.
- Drawing a strong fairness conclusion from a very small per-group sample size — report group
  sizes alongside group-wise metrics, since a gap computed on very few examples is unreliable.

**Debugging tip:** if the reloaded pipeline's predictions don't match the original, check that
the exact same `joblib`/scikit-learn versions are being used to save and load, and that no
additional preprocessing was applied outside the saved pipeline.

**Instructor tip:** have students deliberately save just the final classifier (not the full
pipeline) once, then try to use it on raw unprocessed data, to see the failure mode directly
before contrasting it with saving the full pipeline.
