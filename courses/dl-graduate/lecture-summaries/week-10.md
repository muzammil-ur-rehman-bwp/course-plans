# Week 10 Summary — Deep Reinforcement Learning I: Deep Q-Networks

**Key takeaways:**
- DQN replaces an infeasible $Q$-table with a neural network $Q_\theta(s,a)$, trained to satisfy
  the same Bellman relationship tabular Q-learning already targets (not re-derived here).
- Correlated sequential data, a moving regression target, and function-approximation
  generalization can together cause naive training to diverge.
- Experience replay (uniform random resampling from a buffer) breaks temporal correlation; a
  target network (a periodically-updated frozen copy) stabilizes the moving-target problem.

**You should now be able to:** implement a DQN, a replay buffer, and a target-network update
rule, and explain why each component addresses a specific instability source.

**Next week:** deep reinforcement learning II — policy gradients (REINFORCE, derived) and
actor-critic methods.
