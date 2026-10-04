# Week 3 Summary — Advanced Diffusion Techniques

**Key takeaways:**
- Classifier-free guidance obtains the implicit classifier's score as a difference of a
  conditional and unconditional score from a single jointly-trained network, removing the need
  for a separate classifier; the guidance scale $w$ trades diversity for condition-adherence.
- Flow matching regresses a neural velocity field onto a simple (e.g., linear) interpolation
  path's closed-form velocity — a simulation-free, Jacobian-free alternative to maximum-likelihood
  CNF training.
- Flow matching and the Week 2 probability-flow ODE are both deterministic ODE samplers; flow
  matching allows a broader family of probability paths and a simpler training objective.

**You should now be able to:** derive the classifier-free guidance formula; explain the
diversity/fidelity tradeoff; derive and implement the flow-matching objective and ODE sampler.

**Next week:** Mixture-of-Experts and sparse architectures — the gating/routing formulation,
why sparse activation decouples parameters from per-example compute, and load-balancing.
