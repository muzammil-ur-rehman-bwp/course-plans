# Week 4 — Lecture Content: Contextual Bandits and the Bridge to Reinforcement Learning

## 1. The Contextual Bandit Protocol
At each round t, the learner first observes a context xₜ ∈ ℝ^d (side information — e.g., a
user's features, a query's characteristics), then chooses an arm aₜ from a fixed set, then
observes a reward rₜ drawn from a distribution that depends on **both** xₜ and aₜ. Crucially,
xₜ is drawn independently each round (by nature, or by an oblivious/adaptive adversary over
contexts) and is **not** influenced by the learner's past actions. The learner's goal is still to
minimize regret, now against the best *context-dependent* policy π*(x) = argmaxₐ E[r | x, a], not
against a single best fixed arm — the Week 3 bandit is the special case where the context space
is a single point.

## 2. A Simplified Linear Contextual Bandit (LinUCB-Style)
Assume E[r | x, a] = x · θₐ for an unknown per-arm weight vector θₐ. Maintain, for each arm a, a
running ridge-regression estimate θ̂ₐ from the (context, reward) pairs observed when a was
chosen, together with a UCB-style confidence bonus derived from the regression's uncertainty
(wider when fewer/less-diverse contexts have been observed for that arm):
```python
import numpy as np

class LinUCBArm:
    def __init__(self, d, alpha=1.0, reg=1.0):
        self.A = reg * np.eye(d)   # ridge design matrix, starts at reg * I
        self.b = np.zeros(d)
        self.alpha = alpha

    def score(self, x):
        A_inv = np.linalg.inv(self.A)
        theta_hat = A_inv @ self.b
        bonus = self.alpha * np.sqrt(x @ A_inv @ x)
        return float(x @ theta_hat + bonus)

    def update(self, x, reward):
        self.A += np.outer(x, x)
        self.b += reward * x

def lin_ucb_choose(arms, x):
    return max(range(len(arms)), key=lambda a: arms[a].score(x))
```
This is the direct contextual generalization of Week 3's UCB1: the confidence bonus again
shrinks as more (and more diverse) evidence accumulates for an arm, and again the algorithm acts
optimistically with respect to its current uncertainty.

## 3. The Bandit → Contextual Bandit → MDP Spectrum
| Setting | What varies each round | Does the learner's action affect the future? | Reward depends on | Graduate-course analogue |
|---|---|---|---|---|
| Context-free bandit (Week 3) | Nothing — the same K arms every round | No | Only the chosen arm | A single MDP state with no transitions |
| Contextual bandit (this week) | Context xₜ, drawn independently | No — xₜ₊₁ does not depend on aₜ | Context *and* chosen arm | An MDP with i.i.d.-resetting states and γ effectively 0 beyond one step |
| Full MDP (graduate course) | The state sₜ, which the agent's own past actions influence via the transition model | **Yes** — aₜ changes the distribution of sₜ₊₁ | Current state, action, and all future discounted reward | The full formalism |

The single sharpest distinction is **state persistence under the agent's own control**: in a
contextual bandit, next round's context is handed to the learner by the world, regardless of
which arm it chose; in a full MDP, the agent's action is one of the causes of the next state,
which is exactly why MDPs need a transition model P(s'|s,a) and a multi-step Bellman equation,
while contextual bandits need neither — each round is a fresh, single-step decision problem.

## 4. Where Tabular Q-Learning Sits on This Spectrum
Recall (not re-derive) the graduate course's Q-learning update:
```
Q(s, a) ← Q(s, a) + α [ r + γ max_a' Q(s', a') − Q(s, a) ]
```
The term γ·max_a' Q(s', a') is precisely what a contextual bandit never needs: it is a
*bootstrapped estimate of future value*, necessary only because the agent's action at s causally
determines which s' it then faces. A contextual bandit's analogous "update" would simply be a
one-step regression of reward on (context, arm) — there is no s' term, because there is no
persisting state for the chosen arm to affect. This is why contextual bandits are sometimes
described as "one-step reinforcement learning": they inherit RL's need to generalize across
contexts (unlike the tabular bandit of Week 3) but not RL's need to reason about delayed,
multi-step consequences.

## 5. In-Class/Lab Exercise
Construct a synthetic contextual bandit with d = 4 context features and K = 3 arms, where each
arm's expected reward is a different known linear function of the context (so a ground-truth
regret-zero policy exists). Run the LinUCB-style algorithm above against a context-free UCB1
baseline that ignores the context entirely; plot cumulative regret for both and confirm the
context-aware algorithm's regret grows far more slowly once it has seen enough contexts to
estimate each θₐ accurately.
