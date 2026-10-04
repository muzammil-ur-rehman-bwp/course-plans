# Lab Notes 8 — Quantization and Knowledge Distillation

**Concept recap:** PTQ's affine mapping is set purely from a tensor's observed range (sensitive to
outliers); QAT simulates rounding error during training via a straight-through estimator; KD
trains a student against a temperature-softened teacher distribution plus hard labels.

**Common pitfalls:**
- Using per-tensor (not per-channel) quantization in Task A and being surprised by a large
  accuracy drop at $b=4$ — re-run with `ptq_quantize_per_channel` before concluding $b=4$ PTQ is
  simply unworkable for your architecture.
- In Task B, forgetting that the STE's backward pass must return the incoming gradient unchanged
  — a common bug is accidentally scaling or zeroing the gradient, which silently prevents the
  network from learning anything useful despite the forward pass looking correct.
- In Task C, forgetting the $T^2$ rescaling in `distillation_loss` — omitting it does not break
  training outright but makes the KD-loss term's effective weight implicitly (and confusingly)
  depend on whatever temperature $T$ you chose, making $\alpha$ sweeps hard to interpret.
- Using a teacher that is barely better than the student architecture itself — distillation's
  benefit is easiest to see clearly when there is a genuine capacity gap between teacher and
  student; a near-identical teacher/student pair can make Task C's comparison inconclusive.

**Debugging tip:** before running the full QAT training loop, verify `fake_quantize`'s forward
output exactly matches `ptq_quantize`'s output for the same scale/zero-point on a few sample
tensors — the two should be numerically identical in the forward pass; only the backward behavior
differs.

**Instructor tip:** have students predict, before running Task C, whether the distilled student
will outperform the hard-label student by a large or small margin — then discuss how the margin
depends on how much "dark knowledge" (non-top-class relative confidence) the specific toy task
actually contains; a near-linearly-separable toy task may show a smaller distillation benefit than
a more ambiguous one, which is itself an instructive result.
