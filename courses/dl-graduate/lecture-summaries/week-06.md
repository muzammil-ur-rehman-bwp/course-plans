# Week 6 Summary — Normalizing Flows and the Diffusion Forward Process

**Key takeaways:**
- The change-of-variables formula $p_X(x)=p_Z(f^{-1}(x))\,|\det \partial f^{-1}/\partial x|$ gives
  the exact density of $X=f(Z)$ under an invertible $f$.
- A normalizing flow stacks invertible layers with cheap Jacobian determinants, enabling both
  tractable sampling and exact density evaluation.
- The diffusion forward process is a fixed Markov chain of Gaussian noising steps; its closed-form
  marginal $x_t=\sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\epsilon$ lets any step be sampled
  directly from $x_0$.

**You should now be able to:** derive and implement a 1-D affine normalizing flow, and implement
the diffusion forward process's closed-form sampling at an arbitrary timestep.

**Next week:** advanced generative models II — the diffusion reverse process, the simplified
training objective, and sampling.
