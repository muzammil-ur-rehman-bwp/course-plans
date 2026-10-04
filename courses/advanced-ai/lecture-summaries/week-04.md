# Week 4 Summary — Contextual Bandits and the Bridge to Reinforcement Learning

**Key takeaways:**
- Contextual bandits add side information (context) each round, but the context is still drawn
  independently of the learner's past actions — no state persistence.
- The bandit → contextual bandit → full MDP spectrum turns on exactly one feature: does the
  agent's action causally affect the future state?
- Tabular Q-learning's bootstrapped γ·max Q(s',a') term exists precisely because MDPs have this
  state persistence, which contextual bandits lack.

**You should now be able to:** implement a simplified LinUCB-style contextual bandit; explain
precisely why contextual bandits need no transition model while MDPs do.

**Next week:** Multi-agent reinforcement learning — independent vs. joint-action learners, and
why non-stationarity breaks single-agent convergence guarantees.
