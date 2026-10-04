# Lab Notes 3 — Convolution Arithmetic, Receptive Field, and Pooling

**Concept recap:** output size follows $O = \lfloor (I + 2P - K)/S \rfloor + 1$; receptive field
grows by $(K-1)$ per stacked layer at stride 1; pooling halves spatial size at stride/size 2.

**Common pitfalls:**
- Off-by-one errors from forgetting the floor division in the output-size formula, especially
  when stride does not evenly divide `(input_size + 2*padding - kernel_size)`.
- Forgetting that `nn.Conv2d` expects input shape `(batch, channels, height, width)` — a
  single grayscale image must still carry a channel dimension of 1, not be passed as a bare 2D
  tensor.
- Confusing "padding adds to both sides," so padding of 2 adds 4 total to a dimension, not 2 —
  a frequent source of an output-size calculation being off by exactly 2 per padded dimension.
- Computing receptive field assuming stride 1 throughout when pooling (stride 2) is also present
  — the simple additive formula from lecture only applies to the stride-1 case; pooling layers
  grow the receptive field faster, and this should be reasoned about explicitly, not formula-
  substituted.

**Debugging tip:** when a computed output size does not match `nn.Conv2d`'s actual output shape,
recompute the formula with the exact `padding` and `stride` values passed to the layer — a
mismatched default argument (e.g., assuming `padding=0` when a non-zero value was set) is the
usual cause.

**Instructor tip:** Task B's receptive-field reasoning is often the first time students have to
think about pooling's effect on top of convolution's effect — budget extra discussion time here
rather than treating it as a quick formula plug-in.
