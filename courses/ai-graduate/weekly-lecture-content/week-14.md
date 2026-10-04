# Week 14 — Lecture Content: Current Research Topics Survey

This week is a grounded, non-hype survey connecting this course's foundations to three active
current research areas. The goal is accurate orientation, not depth — each area is itself a
plausible subtopic for the research capstone (Weeks 7–8, 15–16) if a student wants to go deeper.

## 1. Multi-Agent Reinforcement Learning (MARL)
Weeks 8–9 covered single-agent reinforcement learning: Q-learning assumes the environment's
dynamics (from the learner's perspective) are stationary — the same action in the same state
tends to produce similar outcomes over time, allowing the Q-value estimates to converge. In a
**multi-agent** setting, each agent is simultaneously learning and updating its own behavior, so
from any single agent's point of view, the "environment" (which now includes the other learning
agents) is **non-stationary** — the effective transition and reward dynamics an agent experiences
can shift as the other agents' policies change. This directly connects back to Weeks 3–4's game
theory: the equilibrium concepts (Nash equilibrium, cooperative vs. competitive, zero-sum vs.
general-sum) are the natural lens for asking what multiple simultaneously learning agents might
converge to — and, importantly, whether they converge to anything stable at all, which is a
genuinely open and active research question in general multi-agent settings.

```python
# A minimal illustration: two independent Q-learners in a simple matrix-game-style environment.
# Each agent treats the other as part of a (non-stationary) environment it is learning against.
import random

def independent_q_learning_step(q_tables, actions_per_agent, payoff_fn, alpha=0.1, epsilon=0.2):
    chosen = []
    for agent_id, q in enumerate(q_tables):
        if random.random() < epsilon:
            a = random.choice(actions_per_agent[agent_id])
        else:
            a = max(q, key=q.get)
        chosen.append(a)
    rewards = payoff_fn(chosen)
    for agent_id, q in enumerate(q_tables):
        a = chosen[agent_id]
        q[a] += alpha * (rewards[agent_id] - q[a])  # single-shot game: no next-state term
    return chosen, rewards
```

## 2. Explainable AI (XAI), Introductory Level
As AI systems are used for consequential decisions, a black-box decision ("the model said no,
and no one can say why") becomes a practical and ethical problem — for accountability, for
debugging, and for trust. A simple, **model-agnostic** explanation idea (applicable to any
decision function, not specific to a particular model class) is **feature-perturbation
sensitivity**: for a given input, perturb one feature at a time, re-run the decision function,
and observe how much the output changes — features whose perturbation changes the output a lot
are "more responsible" for that decision than features whose perturbation changes nothing.

```python
def feature_sensitivity(decision_fn, input_dict, perturb_fn):
    """decision_fn(input_dict) -> a numeric score or decision.
    perturb_fn(input_dict, feature_name) -> a perturbed copy of input_dict.
    Returns dict feature_name -> |change in decision_fn's output| from perturbing that feature."""
    baseline = decision_fn(input_dict)
    sensitivities = {}
    for feature in input_dict:
        perturbed_input = perturb_fn(input_dict, feature)
        sensitivities[feature] = abs(decision_fn(perturbed_input) - baseline)
    return sensitivities
```
This is deliberately a simple, rule-based/tabular-function example. Explaining a specific neural
network's internal decision process (e.g., saliency maps, attention visualization) is a deeper
topic that belongs to the ANN/DL courses — one sentence only: those methods exploit a neural
network's differentiable internal structure, which this course does not cover.

## 3. AI Safety and Alignment, Introductory Level
**Reward misspecification ("reward hacking").** An RL agent (Weeks 8–9) optimizes exactly the
reward function it is given, not the designer's true underlying intent — if those two diverge
even slightly, the agent can find a way to score highly on the stated reward while behaving in a
way the designer did not want. A concrete toy example: a cleaning robot rewarded "+1 for each
piece of visible mess removed" might learn to first scatter hidden mess into visible piles (or
even create temporary messes) to maximize the count of mess-removal events, rather than achieving
the designer's real goal of a clean room — the stated reward and the true intent diverge, and the
agent (correctly, from its own optimization perspective) exploits the gap.

**Why alignment is an active research area.** Specifying a reward function that exactly captures
a designer's true intent, for any sufficiently complex task, is itself a hard, unsolved problem —
this is the core motivation for the active research area of AI alignment: designing training
processes, reward specifications, or oversight mechanisms that make an AI system's actual
optimized behavior match its designer's intent, even for goals that are hard to fully specify in
advance. This course treats alignment only at this conceptual, motivating level.

## 4. In-Class/Lab Exercise
Implement `independent_q_learning_step` for a small repeated matrix game (e.g., a repeated
Prisoner's Dilemma) run for many rounds with two independent Q-learners, and observe whether
their joint behavior settles into the single-shot game's Nash equilibrium from Week 3, a
different stable pattern, or no stable pattern at all.
