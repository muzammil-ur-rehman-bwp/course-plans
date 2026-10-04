# Week 2 Summary — Score-Based Generative Models: The SDE Formulation

**Key takeaways:**
- The VP-SDE's Euler-Maruyama discretization is exactly DDPM's forward process; the continuous-
  time view is strictly more general.
- The reverse-time SDE reduces sampling to knowing the score function $\nabla_x\log p_t(x)$ at
  every noise level; denoising score matching trains a network to estimate it using the SDE's
  closed-form Gaussian marginal.
- Under the standard reparameterization $s_\theta = -\epsilon_\theta/\sqrt{1-\bar\alpha_t}$,
  denoising score matching's loss is exactly DDPM's noise-prediction loss — discrete-time DDPM is
  a special case of continuous-time score-based generative modeling, not a different model.
- The probability-flow ODE gives the same marginals deterministically, previewing Week 12's fast
  samplers.

**You should now be able to:** derive the Euler-Maruyama discretization of the VP-SDE and match
its coefficients to DDPM; state the reverse-time SDE and the role of the score function; derive
the DDPM-loss-to-score-matching correspondence; implement a toy score network and reverse-SDE
sampler.

**Next week:** Advanced diffusion techniques — classifier-free guidance, derived, and flow
matching/continuous normalizing flows as an alternative continuous-time generative framework.
