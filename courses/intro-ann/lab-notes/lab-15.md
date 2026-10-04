# Lab Notes 15 — Diagnosing Broken Training Runs and Hyperparameter Tuning

**Concept recap:** this lab deliberately reconstructs, from real training curves, the diagnostic
categories (healthy/underfitting/overfitting/broken) and the cheapest-check-first debugging order
covered in lecture.

**Common pitfalls:**
- Making the "learning rate too high" fault *so* extreme that it immediately produces `nan`,
  which is easy to diagnose but less instructive than a moderately-too-high rate that produces a
  visibly oscillating, non-converging curve — tune the fault's severity to be diagnosable but not
  trivial.
- Confusing Task A's "unnormalized inputs" fault with "broken" when it is really closer to
  "severely slowed healthy training" on some datasets/architectures — the exact symptom depends
  on the model and data; discuss the actual observed curve rather than assuming a fixed outcome.
- In Task D, treating training loss (rather than validation loss) as the hyperparameter-selection
  criterion — this is precisely the mistake that would select an overfitting configuration as
  "best."
- Running too few epochs in Task D's sweep for the loss to meaningfully stabilize, making the
  "best" configuration selection noisy — use enough epochs that each configuration's validation
  loss has plausibly stopped improving, or at least plateaued.

**Debugging tip:** when diagnosing a classmate's anonymized curve (Task B), look first at the
*shape* of the gap between training and validation curves (present vs. absent vs. widening) before
looking at the absolute loss values — the gap's shape is more diagnostic than the loss's scale.

**Instructor tip:** Task B's swap-and-diagnose exercise works best with curves from several
different pairs of students, so that the faults being diagnosed are not the student's own
recently-authored bug, which they would recognize immediately rather than diagnose from evidence.
