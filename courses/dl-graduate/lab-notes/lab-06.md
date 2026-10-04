# Lab Notes 6 — A 1-D Normalizing Flow and the Diffusion Forward Process

**Concept recap:** an affine flow's density follows the change-of-variables formula; the
diffusion forward process is a fixed Gaussian noising chain with a closed-form marginal
$x_t=\sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\epsilon$.

**Common pitfalls:**
- Sign errors in the flow's `log_prob`: the correct relation is $\log p_X(x) = \log p_Z(z) -
  \log|\det \partial f/\partial z|$ (subtracting the forward log-determinant), easy to
  accidentally flip to addition, which silently produces an invalid (non-normalized) density that
  can still "train" by gradient descent toward a degenerate solution (e.g., `log_scale` running
  to $-\infty$ to cheat the objective).
- Expecting a **single** affine flow to fit multi-modal data (Task B) — this is a feature of the
  exercise, not a bug: a single affine transform of a unimodal Gaussian is always unimodal
  Gaussian; real flow architectures stack many layers (often with non-affine, data-dependent
  components) to reach multimodal densities.
- **Diffusion noise-schedule bugs:** an incorrectly computed `alpha_bars` (e.g., using `torch.cumsum`
  instead of `torch.cumprod`, or indexing `alpha_bars[t]` off by one relative to `betas[t]`)
  silently produces a forward process that noises data at the wrong rate — always sanity-check
  that `alpha_bars[0]` is close to 1 (almost no noise at $t=0$) and `alpha_bars[T-1]` is close to
  0 (almost pure noise at the final step).
- Forgetting `torch.no_grad()`/detaching when only visualizing forward-process samples (not
  needed for correctness here since there's no backward pass yet, but worth flagging as a habit
  for Lab 7).

**Debugging tip:** if Task D's empirical-vs-closed-form check does not match, print
`alpha_bars[t]` directly and compare against a hand-computed $\prod_{s=1}^t(1-\beta_s)$ for a
small $t$ — an off-by-one in indexing is the most common cause.

**Instructor tip:** Task B's "failure" is intentional — frame it explicitly as motivating why
real flow architectures need more expressive (non-affine) invertible layers, not as a mistake to
fix.
