# Week 5 Summary — Multi-Agent Reinforcement Learning

**Key takeaways:**
- Independent learners run ordinary single-agent Q-learning against each other; joint-action
  learners explicitly model the other agent's action.
- Multi-agent learning makes the environment non-stationary from each agent's perspective,
  breaking the stationarity assumption single-agent Q-learning's convergence proof relies on.
- Self-play trains an agent against copies of itself, providing an automatically scaling
  curriculum as both the agent and its opponents improve together.

**You should now be able to:** implement independent Q-learners in a repeated matrix game;
explain precisely why non-stationarity invalidates single-agent convergence guarantees.

**Next week:** Algorithmic game theory I — the computational complexity of Nash-equilibrium
computation (PPAD-completeness) and correlated equilibria.
