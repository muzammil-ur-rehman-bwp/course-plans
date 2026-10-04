# Week 10 Summary — Partially Observable MDPs (POMDPs)

**Key takeaways:**
- A POMDP extends an MDP with an observation set Ω and observation model Z(o|s',a); the agent
  never directly observes the true state, only a belief state (a probability distribution over
  states).
- The belief update is a two-step Bayesian filter: predict forward with the transition model,
  then reweight by the observation model and normalize.
- Even with a small, finite underlying state space, the belief space is continuous, which is why
  POMDPs are solved by very different (and generally much harder) methods than MDPs.

**You should now be able to:** compute a belief-state update by hand and in Python for a small
POMDP; explain precisely why belief-space continuity makes POMDPs harder than the underlying MDP.

**Next week:** Probabilistic graphical models at a rigorous level — the computational complexity
of exact inference and approximate inference via sampling.
