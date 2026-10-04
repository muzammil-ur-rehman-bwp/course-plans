# Week 5 — Lecture Content: Multi-Agent Reinforcement Learning

## 1. From One Learner to Several
The graduate course's Q-learning convergence guarantee assumes a **stationary** environment: the
transition and reward distributions a given state-action pair produces do not change over time.
That assumption is exactly what breaks when multiple agents learn simultaneously in a shared
environment — from any one agent's point of view, the "environment" now includes the other
agents' policies, which are themselves changing as they learn.

## 2. Independent Learners
The simplest approach: each agent runs ordinary single-agent Q-learning, treating all other
agents as an unmodeled part of the environment.
```python
import random

def independent_q_learning_step(Q, s, a, r, s2, actions, alpha=0.1, gamma=0.9):
    best_next = max(Q[s2][a2] for a2 in actions(s2)) if actions(s2) else 0.0
    Q[s][a] += alpha * (r + gamma * best_next - Q[s][a])

def run_independent_learners(payoff_a, payoff_b, episodes, alpha=0.1, epsilon=0.1, seed=0):
    """payoff_a, payoff_b: 2x2 payoff matrices for a repeated general-sum matrix game
    (rows/cols index each player's two actions)."""
    rng = random.Random(seed)
    actions = [0, 1]
    Qa = {0: 0.0, 1: 0.0}
    Qb = {0: 0.0, 1: 0.0}
    history = []
    for _ in range(episodes):
        a = rng.choice(actions) if rng.random() < epsilon else max(Qa, key=Qa.get)
        b = rng.choice(actions) if rng.random() < epsilon else max(Qb, key=Qb.get)
        ra, rb = payoff_a[a][b], payoff_b[a][b]
        Qa[a] += alpha * (ra - Qa[a])   # single-state repeated game: no s' bootstrap term
        Qb[b] += alpha * (rb - Qb[b])
        history.append((a, b))
    return Qa, Qb, history
```
Each player's update is individually a *correct* single-agent bandit update (this is exactly a
Week 3 bandit, since the "game" here is a single repeated state) — but the reward each player
receives depends on the *other* player's current policy, which keeps shifting.

## 3. Joint-Action Learners
A joint-action learner instead models the other agent(s) explicitly: it conditions its value
estimates on the joint action (or on an estimate of the other agent's current strategy), e.g.
Q(a, b) rather than separate Q(a) and Q(b). This captures strategic interaction that independent
learners structurally cannot represent (an independent learner's Q(a) has no way to express "my
best action depends on what the other player is currently doing"), at the cost of a joint action
space that grows multiplicatively with the number of agents.

## 4. Why Non-Stationarity Breaks the Convergence Argument
Single-agent Q-learning's convergence proof (standard stochastic-approximation theory) relies on
the Bellman backup being applied, in expectation, to a **fixed** target distribution — the same
transition/reward process at every visit to a given (s, a). With independent learners, player A's
effective reward for action a depends on player B's current (changing) policy, so the "target"
player A's Q-update is chasing is itself moving. Two consequences, both observable empirically:
(1) the joint process can fail to converge to any fixed point at all, instead cycling among
several joint action pairs indefinitely (observable directly in the matching-pennies-style game
below, which has no pure-strategy Nash equilibrium — recall this from the graduate course); (2)
even when it does converge, it need not converge to a Nash equilibrium of the stage game, since
neither player's update rule was designed with the other's incentives in mind.

## 5. Self-Play
Self-play trains an agent by having it repeatedly play against copies of itself (often including
past checkpoints, not just the current version, to avoid cycling against a single evolving
target). The key generic mechanism, accurately stated without reference to any single system's
specific unverified implementation details: as the agent improves, its opponent (itself) improves
too, so the difficulty of the opponents it faces automatically tracks its own current skill level
— an automatically scaling curriculum that needs no externally hand-designed difficulty schedule.
This is a standard training paradigm behind many strong game-playing systems trained primarily
through self-play rather than from a fixed, pre-collected dataset of games, though the specific
algorithmic details (how opponents are sampled, what mix of past checkpoints is used) vary
considerably across systems and are an active area of practical engineering, not a single fixed
recipe.

## 6. In-Class/Lab Exercise
Run `run_independent_learners` on a repeated matching-pennies-style zero-sum game (no
pure-strategy Nash equilibrium) and on a repeated coordination game (a pure-strategy Nash
equilibrium exists and is also the social optimum); compare the resulting action-frequency
trajectories, and explain why one setting converges to stable play and the other cycles.
