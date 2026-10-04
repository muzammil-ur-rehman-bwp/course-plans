# Week 14 Summary — Current Research Topics Survey

**Key takeaways:**
- Multi-agent RL is harder than single-agent RL because each agent's environment is
  non-stationary from its own perspective — the other agents are learning too; this connects
  directly to Weeks 3–4's game-theoretic equilibrium concepts.
- Feature-perturbation sensitivity is a simple, model-agnostic XAI technique: perturb one
  feature at a time and observe how much the decision changes.
- Reward misspecification ("reward hacking") happens when an agent optimizes exactly the stated
  reward, which can diverge from the designer's true intent; this motivates the active research
  area of AI alignment.

**You should now be able to:** explain why multi-agent RL is non-stationary from each agent's
perspective; implement a simple feature-perturbation explanation; give a concrete example of
reward misspecification.

**Next week:** Research project work session — structured time for the capstone literature
review and experiment, plus presentation practice with peer feedback.
