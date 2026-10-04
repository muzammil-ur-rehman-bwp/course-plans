# Week 9 — Lecture Content: Markov Decision Processes II

*(Midterm Exam covers Weeks 1–8; this content is delivered in the remainder of Week 9's session.)*

## 1. Policy Iteration
Policy iteration alternates two steps until the policy stops changing:

**Policy evaluation.** Given a fixed policy π, solve for V^π exactly (or approximately, via
repeated backups) by applying the *policy-specific* Bellman equation (no max over actions, since
the action is fixed by π):
```
V^π(s) = Σ_s' P(s' | s, π(s)) [ R(s, π(s), s') + γ V^π(s') ]
```

**Policy improvement.** Given V^π, compute a new, greedy policy:
```
π'(s) = argmax_a  Σ_s' P(s' | s, a) [ R(s, a, s') + γ V^π(s') ]
```

```python
def policy_evaluation(policy, states, transition_model, reward, gamma, theta=1e-6):
    V = {s: 0.0 for s in states}
    while True:
        delta = 0.0
        for s in states:
            if s not in policy:
                continue
            a = policy[s]
            new_v = sum(p * (reward(s, a, s2) + gamma * V[s2])
                        for p, s2 in transition_model(s, a))
            delta = max(delta, abs(new_v - V[s]))
            V[s] = new_v
        if delta < theta:
            return V

def policy_iteration(states, actions, transition_model, reward, gamma):
    policy = {s: actions(s)[0] for s in states if actions(s)}
    while True:
        V = policy_evaluation(policy, states, transition_model, reward, gamma)
        stable = True
        for s in states:
            if not actions(s):
                continue
            best_a = max(actions(s), key=lambda a: sum(
                p * (reward(s, a, s2) + gamma * V[s2]) for p, s2 in transition_model(s, a)))
            if best_a != policy[s]:
                policy[s] = best_a
                stable = False
        if stable:
            return policy, V
```

**Why policy iteration converges in finitely many iterations.** For a finite MDP there are only
finitely many distinct deterministic policies. Each policy-improvement step either leaves the
policy unchanged (in which case it is already optimal — a fixed point of the Bellman optimality
operator, by construction of the greedy improvement step) or strictly improves V^π at some
state without making it worse at any other state (a standard monotonic-improvement lemma). Since
V^π strictly improves and there are only finitely many policies, the sequence of policies visited
cannot repeat and must terminate at the optimal policy in a finite number of steps — unlike value
iteration, which converges only in the limit (asymptotically), policy iteration provably reaches
the exact optimal policy after finitely many full evaluation/improvement rounds.

## 2. The Exploration-Exploitation Tradeoff
Value iteration and policy iteration both assume the transition model P and reward function R
are *known*. Reinforcement learning drops this assumption: the agent must learn a good policy
purely from experience (observed transitions and rewards). This creates a fundamental tradeoff:
**exploitation** (take the action currently believed best, to maximize reward now) vs.
**exploration** (try a possibly-suboptimal action to gather more information, which might reveal
a better long-run policy). An agent that only exploits can get permanently stuck acting on an
early, inaccurate estimate and never discover a better action whose value it never bothered to
sample. The standard, simple way to balance this is an **ε-greedy** policy: with probability
1 − ε take the currently-best-known action; with probability ε take a uniformly random action.

## 3. Q-Learning
Q-learning learns the **action-value function** Q(s, a) — the expected discounted return of
taking action a in state s and then acting optimally thereafter — directly from sampled
transitions, without ever needing to know P or R explicitly (it is **model-free**). After
observing a transition (s, a, r, s'), the Q-learning update is:
```
Q(s, a) ← Q(s, a) + α [ r + γ max_a' Q(s', a') − Q(s, a) ]
```
where α ∈ (0, 1] is the learning rate. The term in brackets is the **TD (temporal-difference)
error**: the difference between the current estimate Q(s,a) and a one-step-lookahead target
r + γ·max_a' Q(s', a'). Q-learning is **off-policy**: the update uses max_a' Q(s', a') (the best
action under the current estimate), regardless of which action the exploration policy (e.g.,
ε-greedy) actually took next — this is what lets it learn the optimal Q-function even while
behaving partly randomly for exploration.

```python
import random

def q_learning(states, actions, step_env, episodes, alpha=0.1, gamma=0.9, epsilon=0.1):
    """step_env(s, a) -> (reward, next_state, done). actions(s) -> list of legal actions."""
    Q = {s: {a: 0.0 for a in actions(s)} for s in states if actions(s)}
    for _ in range(episodes):
        s = random.choice([s for s in states if actions(s)])
        done = False
        while not done:
            if random.random() < epsilon:
                a = random.choice(actions(s))
            else:
                a = max(Q[s], key=Q[s].get)
            r, s2, done = step_env(s, a)
            best_next = 0.0 if done or s2 not in Q else max(Q[s2].values())
            Q[s][a] += alpha * (r + gamma * best_next - Q[s][a])
            s = s2
    return Q
```

## 4. Model-Based vs. Model-Free, Summarized
| Method | Needs P, R known? | Converges to | Guarantee type |
|---|---|---|---|
| Value iteration | Yes | V* (exactly, via a known model) | Asymptotic (contraction) |
| Policy iteration | Yes | π* (exactly) | Finite number of iterations |
| Q-learning | No (learns from samples) | Q* (hence π*), under standard conditions (sufficient exploration, decaying α) | Asymptotic, stochastic-approximation guarantee |

## 5. In-Class/Lab Exercise
Run tabular Q-learning on the Week 8 grid-world for a few thousand episodes with ε = 0.1 and
again with ε = 0 (pure exploitation from an all-zero initial Q-table); compare the learned
policy's quality and discuss what went wrong in the ε = 0 run.
