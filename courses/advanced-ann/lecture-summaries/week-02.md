# Week 2 Summary — The Neural Tangent Kernel Revisited in Depth

**Key takeaways:**
- Tracking function values under gradient flow gives the exact relation $\dot u_t = -\Theta_t u_t$
  at any width; the NTK result is that $\Theta_t \approx \Theta_0 \to \Theta^\infty$ (deterministic
  and fixed) as width $\to\infty$, turning training into linear kernel gradient descent.
- "Lazy training" names the mechanism precisely: relative parameter movement
  $\|\theta_t-\theta_0\|/\|\theta_0\|\to 0$ as width grows, which is why the kernel barely moves.
- The sharper critique: no feature learning by construction, often-loose NTK-regime generalization
  bounds, and measurable kernel drift at practically used, finite widths.

**You should now be able to:** derive the function-space ODE from gradient flow; state the lazy-
training condition precisely; critique NTK theory's limits with specific, named mechanisms rather
than a vague "it's only infinite width" objection.

**Next week:** Mean-field theory of neural networks — the width→∞ limit via the distribution of
weights rather than a fixed kernel, contrasted with this week's NTK limit, and connected back to
initialization-theory signal propagation.
