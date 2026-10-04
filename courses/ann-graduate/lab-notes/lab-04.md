# Lab Notes 4 — Initialization Variance Experiment

**Concept recap:** Xavier/Glorot initialization is derived to preserve variance *on average*, for
activations that are approximately linear near zero (tanh); He initialization corrects
specifically for ReLU zeroing roughly half its inputs. Neither derivation is exact for every
real activation or every real input distribution — they are first-order arguments, confirmed
empirically here, not guarantees of perfectly flat variance in every finite run.

**Common pitfalls:**
- Using He's formula with tanh (or Xavier's with ReLU) in Task A/B and being confused by the
  mismatch — the two formulas are specifically *paired* with specific activations; mixing them
  is a common error students make when generalizing too quickly from the lecture.
- Measuring variance over too few samples (`n_samples` too small), producing a noisy variance
  estimate that obscures the clean exponential-decay/growth pattern the naive scheme should show.
- In Task D, forgetting that backpropagating gradient variance requires a *fresh* backward pass
  per layer depth tested, and that the "gradient" here is with respect to activations, not
  weights — a common mix-up with the Week 7 optimizer labs' weight-gradient focus.
- Reporting the Task C ratio without stating depth 1 vs. depth 30 explicitly — a ratio alone,
  without stating which two layers it compares, is not a gradable, checkable claim.

**Debugging tip:** if naive initialization's variance does not visibly explode or collapse over
30 layers, the width is likely too small for the geometric effect to show clearly within 30
layers, or the random seed produced an atypical run — increase width or depth, or average over
several seeds.

**Instructor tip:** Task D is the lab's most valuable addition beyond the lecture — many students
assume (incorrectly) that initialization theory is "only about the forward pass" until they see
the symmetric effect on backward gradient variance directly.
