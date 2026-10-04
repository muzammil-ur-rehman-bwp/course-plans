# Lab Notes 4 — A Small CNN With a Residual Block on CIFAR-10

**Concept recap:** a residual block adds its input back to its output (`out + identity`); this
requires the output shape to exactly match the input shape, or a projection shortcut is needed.

**Common pitfalls:**
- Adding a residual connection when the block changes the number of channels or spatial size
  without a matching projection (e.g., a 1×1 convolution) on the shortcut path — this causes a
  shape-mismatch error at the addition step.
- Forgetting `model.eval()` when computing validation/test accuracy, which leaves `BatchNorm2d`
  using batch statistics on the (often differently-sized) validation batch, producing
  inconsistent accuracy numbers between runs.
- Giving the plain and residual CNNs in Task B meaningfully different parameter counts or depths,
  which confounds the comparison — match them as closely as reasonably possible except for the
  skip connection itself.
- CIFAR-10 download failures in restricted/offline environments — fall back to Fashion-MNIST
  (adjusting `in_channels` from 3 to 1) rather than blocking on dataset access.

**Debugging tip:** if training loss for the residual model does not look any better than the
plain model's, check that the skip connection's addition is actually wired into the forward pass
(a common copy-paste bug is defining `self.res_block` but never calling it, or calling it without
using its output).

**Instructor tip:** Task D's discussion is this lab's conceptual payoff — on a small CNN at this
depth, the residual version may not show a dramatic difference over the plain version; frame this
as an opportunity to discuss that skip connections matter *more* as depth increases, not as a
failed experiment.
