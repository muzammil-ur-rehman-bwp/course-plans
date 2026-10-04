# Lab Notes 13 — Convolution, Pooling, and a Small CNN

**Concept recap:** convolution computes a local weighted sum with a shared kernel at every
position; pooling summarizes small regions, most commonly by taking the max; a CNN chains
several conv/pool stages before a final dense classification layer.

**Common pitfalls:**
- Off-by-one errors in `conv2d`'s output-size formula — double check
  `(input_size - kernel_size) // stride + 1` against the actual loop bounds; a mismatch causes an
  `IndexError` or a silently truncated output.
- Confusing a convolution kernel's mathematical flip (true "convolution" flips the kernel; most
  deep learning frameworks and this lab's `conv2d` actually implement **cross-correlation**, i.e.
  no flip) — this is a standard, intentional simplification in practice and does not affect
  anything the network learns, but is worth naming explicitly if a student compares against a
  signal-processing textbook's strict convolution definition.
- In Task C, forgetting that `nn.Conv2d` expects input shape `(batch, channels, height, width)` —
  MNIST images loaded as `(batch, 1, 28, 28)` already match this; do not flatten them as in Week
  12's MLP lab.
- Comparing Task D's CNN and MLP unfairly (e.g., very different training epochs or learning
  rates) — hold the training budget as similar as reasonably possible for the comparison to be
  meaningful.

**Debugging tip:** visualize Task A's convolution outputs as images (not just printed arrays) —
an edge-detector kernel should visibly highlight edges, and a blur kernel should visibly smooth
the image; if neither pattern appears, the kernel or the convolution implementation likely has a
bug.

**Instructor tip:** Task D's side-by-side comparison is this week's conceptual payoff — budget
time to discuss it as a class rather than letting it become just another notebook cell.
