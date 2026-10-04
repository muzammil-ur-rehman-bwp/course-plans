# Lab Notes 2 — Advanced CNN Building Blocks

**Concept recap:** a residual block adds its input back (`out + identity`, shape-preserving); a
dense block concatenates every preceding layer's output within the block (growing channel count);
a depthwise separable convolution factors a standard convolution into a per-channel depthwise
step plus a channel-mixing pointwise step.

**Common pitfalls:**
- Confusing DenseNet's **concatenation** with ResNet's **addition** — writing
  `torch.cat([x, out])` where a `+` was intended, or vice versa, silently produces wrong channel
  counts downstream (concatenation grows channels; addition requires matching channels and keeps
  the count fixed).
- Forgetting to update the running channel count when stacking multiple `DenseLayer`s manually
  (each layer's input channel count must include every previous layer's growth-rate contribution).
- Using `groups=in_channels` incorrectly in the depthwise step (e.g., mismatched `in_channels`
  between the depthwise and pointwise stage) — `Conv2d(groups=C)` requires `in_channels % groups
  == 0`, and a depthwise layer's `out_channels` must equal `in_channels` (one filter per channel)
  before the pointwise stage changes the channel count.
- Reporting a depthwise-separable parameter-count ratio that doesn't match the
  $1/C_{out}+1/k^2$ formula — usually caused by comparing against a standard `Conv2d` with a
  different `in_channels`/`out_channels` configuration than the separable version actually used.

**Debugging tip:** if a `DenseBlock`'s final output channel count doesn't match
$C_{in} + k\cdot\text{num\_layers}$, print each `DenseLayer`'s input and output channel count in
sequence — the bug is almost always in how the running channel count was threaded between layers.

**Instructor tip:** Task D's "assemble all three blocks" exercise is intentionally a shape-wiring
exercise as much as a training exercise — most Task D failures are channel-count mismatches at
block boundaries, not training/optimization issues.
