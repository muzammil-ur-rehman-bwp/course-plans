# Lab Notes 14 — Multi-Agent Q-Learning & Feature-Sensitivity Explanation

**Concept recap:** in multi-agent RL, each agent's environment is non-stationary because the
other agents are learning too; feature-perturbation sensitivity is a simple model-agnostic
explanation technique that perturbs one feature at a time and measures the output's change.

**Common pitfalls:**
- In Task A, using a *shared* Q-table between the two agents instead of two independent ones —
  the whole point of "independent" Q-learning is that each agent only sees its own experience
  and has no access to the other's internal estimates.
- Expecting the two independent Q-learners to reliably converge to the single-shot Nash
  equilibrium — in a repeated game with independent learners this is not guaranteed in general;
  Task B asks students to *report* the observed frequencies, not to assume a particular
  theoretical outcome in advance.
- In Task C, perturbing all features simultaneously instead of one at a time — this defeats the
  purpose of isolating each feature's individual sensitivity, exactly analogous to the Week 13
  ablation-study confound.
- In Task D, describing a reward-hacking example where the "exploit" is actually just the
  intended behavior (e.g., "the agent cleaned the room, which is what we wanted") — a valid
  example must show a genuine gap between the literal reward signal and the designer's actual
  intent.

**Debugging tip:** for Task A/B, log the running average payoff per agent over rounds, not just
the final 100 rounds' action frequencies — this reveals whether the system has actually settled
into a stable pattern or is still drifting.

**Instructor tip:** connect Task A/B explicitly back to the Week 3 lab's Nash-equilibrium
finder — ask students whether the behavior they observe from two *learning* agents matches, or
diverges from, the equilibrium concept computed analytically for the same game.
