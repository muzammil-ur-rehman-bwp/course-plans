# Lab Notes 11 — Gradient Flow and Attention Expressivity

**Concept recap:** a residual block's advantage comes specifically from starting **near** the
identity map; the further its branch's initialization is pushed from "outputs near zero," the
less the Jacobian stays close to $I$, and the less pronounced the gradient-flow advantage over a
plain network becomes.

**Common pitfalls:**
- Forgetting to zero-initialize the residual branch's last layer in the baseline comparison
  (Task A) — if both the plain and residual network start from the same generic random
  initialization, the residual network's advertised advantage will not show up as cleanly, since
  the whole argument depends on starting near identity.
- In Task B, changing *multiple* things between initialization scales (e.g., also changing the
  learning rate or depth) and attributing the resulting gradient-norm change entirely to the
  initialization scale — isolate one variable at a time.
- In Task C, forgetting the softmax normalization in the attention-weight computation, which
  produces a "weight matrix" that is not a valid probability distribution per row and makes the
  heatmap uninterpretable as attention weights.
- In Task D, confusing "path length" (how many layers' worth of computation separate two
  positions) with "receptive field size" (how many input positions affect one output position) —
  related but distinct quantities; the lecture's argument is specifically about path length.

**Debugging tip:** if Task A shows the residual network's gradient norm also shrinking
significantly with depth (even if less than the plain network's), double-check the residual
branch's last layer is actually initialized to all-zero weights *and* zero bias, not just small
values.

**Instructor tip:** Task D's hand computation (3 layers of kernel-size-3 convolution needed to
span a receptive field reaching 20 positions away, roughly, versus attention's single layer) is a
fast, concrete way to make the expressivity argument land without any architecture-depth content.
