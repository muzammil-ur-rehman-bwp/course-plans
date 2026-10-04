# Lab Notes 8 — Double Descent, Weight Decay, and Dropout

**Concept recap:** the interpolation threshold occurs where model capacity first becomes just
large enough to fit the training set exactly (here, `width == n_train`); double descent's test-
error peak sits at or near that threshold, with test error falling again on both the under- and
over-parameterized sides as capacity moves away from it in either direction.

**Common pitfalls:**
- **Misinterpreting the double-descent curve's peak as "the best model."** The peak is the
  *worst* point in the curve (highest test error), not a target — the classical U-shaped
  intuition can tempt students to read the first local rise as "approaching overfitting, so stop
  here," which is backwards for the second (over-parameterized) descending region of the curve.
- Reading the curve only up to the peak and concluding "capacity hurts generalization" without
  continuing the width sweep far enough past the threshold to see the second descent — a
  truncated sweep can look just like the classical U and miss the entire point of the exercise.
- Confusing the interpolation threshold with "the number of parameters equals the number of
  training examples" in general — here it is literally `width == n_train` because of how the
  random-feature construction is set up, but for a real network, the precise threshold depends
  on the effective capacity of the architecture, not simply raw parameter count.
- In Tasks C/D, using a learning rate or epoch count too small for the *regularized* runs to
  actually converge, making a comparison that is really about under-training, not about the
  regularizer's effect.

**Debugging tip:** if Task A's curve shows no visible peak at all, check that the width sweep
actually passes through `n_train` with fine enough granularity near the threshold (the effect can
be sharp and easy to step over with a coarse sweep).

**Instructor tip:** explicitly ask students, during the lab, to point to the double-descent
curve's peak and state out loud whether it is the best or worst point — this one question surfaces
the single most common misreading of this plot quickly.
