# Week 10 — Lecture Content: Deep Reinforcement Learning I — Deep Q-Networks

## 1. From Tables to Networks (Not Re-Deriving the Foundations)

*Artificial Intelligence*, Graduate established, for a **finite** state/action MDP: the Bellman
optimality equation $Q^*(s,a) = \mathbb{E}\left[r + \gamma\max_{a'}Q^*(s',a')\right]$, and tabular
Q-learning's update $Q(s,a)\leftarrow Q(s,a)+\alpha\big[r+\gamma\max_{a'}Q(s',a')-Q(s,a)\big]$,
which requires one table entry per $(s,a)$ pair. **This course does not re-derive either.** When
the state space is large or continuous (e.g., raw pixels, or a continuous sensor vector), an
explicit table is infeasible, so a **Deep Q-Network (DQN)** instead approximates $Q(s,a)$ with a
neural network $Q_\theta(s,a)$, trained so its output satisfies the same Bellman relationship the
tabular update already targets.

## 2. The DQN Loss

Treat the Bellman equation as a regression target. For a sampled transition $(s,a,r,s')$, define
the **target**

$$
y = r + \gamma \max_{a'} Q_{\theta^-}(s', a'),
$$

(where $Q_{\theta^-}$ is the **target network**, Section 4) and train $Q_\theta$ by minimizing the
squared error between its prediction and this target:

$$
\mathcal{L}(\theta) = \mathbb{E}_{(s,a,r,s')\sim \mathcal D}\Big[\big(y - Q_\theta(s,a)\big)^2\Big].
$$

This is exactly the tabular Q-learning update's fixed point, re-expressed as a loss a neural
network can be trained on by ordinary gradient descent — the "target" plays the role of a label in
a (moving) supervised regression problem.

## 3. Why Naive Function Approximation Can Diverge

Three factors, present together, can destabilize training if left unaddressed:

1. **Correlated, non-stationary data:** consecutive transitions from one trajectory are highly
   correlated (similar states in a row), violating the i.i.d.-data assumption behind ordinary
   SGD convergence arguments, and can cause the network to "forget" earlier experience as it
   overfits to whatever region of state space it is currently visiting.
2. **A moving target:** because $Q_\theta$ appears on *both* sides of the loss (the target
   $y$ is itself computed from a $Q$-network), every gradient step that changes $\theta$ also
   changes the targets for every other state — "chasing a moving target" rather than regressing
   toward a fixed label, which can oscillate or diverge.
3. **Bootstrapping + function approximation:** with a table, an update to $Q(s,a)$ cannot affect
   any other entry; with a function approximator, updating $\theta$ to fit $(s,a)$ can
   *inadvertently* shift $Q_\theta$'s value at *other* states too (generalization across similar
   inputs), which can compound the moving-target problem.

## 4. Experience Replay and Target Networks: The Fixes

- **Experience replay:** store transitions $(s,a,r,s')$ in a large buffer $\mathcal D$ as they are
  collected, and train on **uniformly random minibatches sampled from the buffer**, rather than on
  consecutive transitions in order. This breaks temporal correlation (addressing factor 1) and
  lets each transition be reused many times, improving sample efficiency.
- **Target network:** maintain a second copy of the network, $Q_{\theta^-}$, whose weights are
  **frozen** for many steps and only periodically copied from (or slowly blended with) $\theta$.
  Because the target $y$ in Section 2 is computed from $\theta^-$, not the constantly-updating
  $\theta$, the regression target stays fixed for many consecutive updates — directly addressing
  the moving-target problem (factor 2).

```python
import random
from collections import deque
import torch
import torch.nn as nn

class QNetwork(nn.Module):
    def __init__(self, state_dim, num_actions, hidden=64):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(state_dim, hidden), nn.ReLU(),
            nn.Linear(hidden, hidden), nn.ReLU(),
            nn.Linear(hidden, num_actions),
        )

    def forward(self, s):
        return self.net(s)

class ReplayBuffer:
    def __init__(self, capacity=10_000):
        self.buffer = deque(maxlen=capacity)

    def push(self, s, a, r, s_next, done):
        self.buffer.append((s, a, r, s_next, done))

    def sample(self, batch_size):
        batch = random.sample(self.buffer, batch_size)
        s, a, r, s_next, done = zip(*batch)
        return (torch.stack(s), torch.tensor(a), torch.tensor(r, dtype=torch.float32),
                torch.stack(s_next), torch.tensor(done, dtype=torch.float32))

def dqn_update(q_net, target_net, buffer, optimizer, batch_size=64, gamma=0.99):
    if len(buffer.buffer) < batch_size:
        return None
    s, a, r, s_next, done = buffer.sample(batch_size)
    q_values = q_net(s).gather(1, a.view(-1, 1)).squeeze(1)
    with torch.no_grad():
        max_next_q = target_net(s_next).max(dim=1).values
        target = r + gamma * (1 - done) * max_next_q
    loss = nn.functional.mse_loss(q_values, target)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    return loss.item()

def update_target_network(q_net, target_net):
    target_net.load_state_dict(q_net.state_dict())   # hard update; soft (Polyak) update is an alternative
```

## 5. In-Class Exercise

Given $\gamma=0.9$, a transition with $r=1$, and $\max_{a'}Q_{\theta^-}(s',a') = 5.0$, compute the
DQN target $y$. Then, given $Q_\theta(s,a)=4.5$, compute the squared-error loss term by hand.
