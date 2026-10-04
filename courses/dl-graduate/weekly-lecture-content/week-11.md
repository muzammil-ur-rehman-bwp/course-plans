# Week 11 — Lecture Content: Deep Reinforcement Learning II — Policy Gradient and Actor-Critic

## 1. Policy Gradient Methods: A Different Target of Learning

DQN (Week 10) learns a value function and acts greedily with respect to it. **Policy gradient**
methods instead directly parameterize a (typically stochastic) policy $\pi_\theta(a\mid s)$ —
e.g., a neural network outputting action probabilities — and optimize $\theta$ to maximize
expected return directly, with no value function required at all.

## 2. The REINFORCE Gradient Estimator, Derived

Let $\tau = (s_0,a_0,s_1,a_1,\ldots)$ denote a trajectory, $P_\theta(\tau)$ its probability under
policy $\pi_\theta$, and $R(\tau)$ its total return. The learning objective is

$$
J(\theta) = \mathbb{E}_{\tau\sim P_\theta}[R(\tau)] = \sum_\tau P_\theta(\tau)R(\tau).
$$

Differentiate directly:

$$
\nabla_\theta J(\theta) = \sum_\tau \nabla_\theta P_\theta(\tau)\, R(\tau).
$$

This cannot be evaluated as an expectation yet, because it is weighted by $\nabla_\theta
P_\theta(\tau)$, not by $P_\theta(\tau)$ itself. Apply the **log-derivative ("score function")
trick**, $\nabla_\theta P_\theta(\tau) = P_\theta(\tau)\nabla_\theta \log P_\theta(\tau)$:

$$
\nabla_\theta J(\theta) = \sum_\tau P_\theta(\tau)\,\nabla_\theta \log P_\theta(\tau)\, R(\tau) =
\mathbb{E}_{\tau\sim P_\theta}\big[\nabla_\theta \log P_\theta(\tau)\, R(\tau)\big],
$$

which *is* an expectation, estimable by sampling trajectories. Since the environment's transition
probabilities do not depend on $\theta$, $\log P_\theta(\tau) = \sum_t \log \pi_\theta(a_t\mid
s_t) + (\theta\text{-independent terms})$, so $\nabla_\theta \log P_\theta(\tau) = \sum_t
\nabla_\theta\log\pi_\theta(a_t\mid s_t)$ — the environment's unknown dynamics drop out entirely.
Substituting, and writing $G_t$ for the return-from-time-$t$ (which can replace the full-trajectory
$R(\tau)$ by the standard causality argument — future actions cannot affect past rewards, so each
term only needs the return *from* that point on):

$$
\boxed{\ \nabla_\theta J(\theta) = \mathbb{E}_{\pi_\theta}\left[\sum_t \nabla_\theta
\log\pi_\theta(a_t\mid s_t)\, G_t\right]\ }
$$

This is the **REINFORCE** estimator: sample a trajectory, and for every time step, push up the
log-probability of the action actually taken, scaled by how good the return turned out to be from
that point on.

```python
import torch
import torch.nn as nn

class PolicyNetwork(nn.Module):
    def __init__(self, state_dim, num_actions, hidden=64):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(state_dim, hidden), nn.ReLU(),
            nn.Linear(hidden, num_actions),
        )

    def forward(self, s):
        return torch.softmax(self.net(s), dim=-1)

def compute_returns(rewards, gamma=0.99):
    returns = []
    G = 0.0
    for r in reversed(rewards):
        G = r + gamma * G
        returns.insert(0, G)
    return torch.tensor(returns, dtype=torch.float32)

def reinforce_update(policy, optimizer, states, actions, rewards, gamma=0.99, baseline=True):
    returns = compute_returns(rewards, gamma)
    if baseline:
        returns = returns - returns.mean()           # subtract a simple baseline (Section 3)
    probs = policy(torch.stack(states))
    log_probs = torch.log(probs.gather(1, torch.tensor(actions).view(-1, 1)).squeeze(1) + 1e-8)
    loss = -(log_probs * returns).mean()              # negative: we ascend J, so descend -J
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    return loss.item()
```

## 3. Variance Reduction: Baselines

The raw estimator $\sum_t \nabla_\theta\log\pi_\theta(a_t\mid s_t)\,G_t$ is unbiased but can have
very high variance (returns $G_t$ can vary wildly across trajectories/episodes for reasons
unrelated to the quality of a particular action). Subtracting any baseline $b(s_t)$ that does not
depend on the action taken leaves the estimator **unbiased** — because
$\mathbb{E}_{a_t\sim\pi_\theta}[\nabla_\theta\log\pi_\theta(a_t\mid s_t)]\,b(s_t) = b(s_t)\cdot
\nabla_\theta\sum_a \pi_\theta(a\mid s_t) = b(s_t)\cdot\nabla_\theta 1 = 0$ — while typically
reducing its variance substantially when $b(s_t)$ tracks the expected return from $s_t$:

$$
\nabla_\theta J(\theta) = \mathbb{E}_{\pi_\theta}\left[\sum_t \nabla_\theta\log\pi_\theta(a_t\mid
s_t)\, \big(G_t - b(s_t)\big)\right].
$$

A simple choice is the batch-average return (used in the code above); a better choice is a learned
state-value estimate, which leads directly to actor-critic methods.

## 4. Actor-Critic Methods (Conceptual)

Instead of a simple scalar baseline, **learn** a value function $V_\phi(s)$ (the **critic**) via
ordinary regression toward observed returns (or a bootstrapped TD target, as in Week 10's
Bellman-style regression), and use it to form the **advantage**

$$
A(s_t,a_t) = G_t - V_\phi(s_t),
$$

which replaces $G_t - b(s_t)$ above. The **actor** (the policy $\pi_\theta$) is updated using
$\nabla_\theta\log\pi_\theta(a_t\mid s_t)\,A(s_t,a_t)$; the critic is updated to reduce its own
value-prediction error. Because $V_\phi$ adapts to the specific state distribution being visited
(rather than being one global scalar), the advantage estimate is typically much lower-variance
than a plain return-minus-batch-mean baseline, which is why actor-critic methods generally train
more stably and sample-efficiently than plain REINFORCE on non-trivial tasks.

## 5. In-Class Exercise

For a 3-step episode with rewards $r_1=1, r_2=0, r_3=2$ and $\gamma=0.9$, compute $G_1, G_2, G_3$
by hand using the recursive definition $G_t = r_t + \gamma G_{t+1}$.
