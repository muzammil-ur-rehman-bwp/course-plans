# Week 11 Summary — Scaling Laws from a Systems/Engineering Perspective

**Key takeaways:**
- This week takes empirical power-law scaling as given and asks how to allocate a fixed compute
  budget in practice — distinct from the sibling *Advanced Artificial Neural Network* course's
  theoretical "why do power laws hold" question.
- Using $C\approx 6ND$, compute-optimal allocation grows $N$ and $D$ together at comparable,
  fitted-exponent rates, not $N$ alone — over-sizing a model relative to its training-token count
  wastes compute.
- Checkpointing trades checkpoint overhead against expected lost compute on failure; the optimal
  interval is $\tau^*=\sqrt{2c/\lambda}$.
- At large-scale training's node-count and duration, hardware failure is near-certain, so
  production systems are engineered around elastic, fault-tolerant restart.

**You should now be able to:** compute a compute-optimal $(N,D)$ allocation; state precisely how
this differs from the theoretical scaling-laws question; derive and compute the optimal
checkpoint interval.

**Next week:** A grounded, fast-moving-flagged survey of current frontier generative-modeling
research — fast samplers and distillation of diffusion models.
