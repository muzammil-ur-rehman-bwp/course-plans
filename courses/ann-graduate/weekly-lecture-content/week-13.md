# Week 13 — Lecture Content: The Lottery Ticket Hypothesis and Pruning

## 1. The Hypothesis
Frankle and Carbin's **Lottery Ticket Hypothesis**: a randomly-initialized, dense feedforward
network contains a sparse subnetwork ("winning ticket") that, when trained **in isolation, from
that same original initialization**, can match the full dense network's test accuracy in a
comparable number of training iterations — despite having only a small fraction of the original
parameters. The name alludes to the idea that a large network's many possible sparse subnetworks
are like many lottery tickets, and training the full dense network is an efficient way to find one
that "wins" (trains well), even though that same sparse structure, started from a *different*
random initialization, typically fails to train nearly as well.

## 2. Iterative Magnitude Pruning
The standard procedure used to find a winning ticket:
1. Initialize a dense network with initial weights $\theta_0$.
2. Train it to convergence, obtaining trained weights $\theta_T$.
3. **Prune**: remove (zero out, permanently) some fraction of weights with the smallest magnitude
   in $\theta_T$ (commonly per-layer, to keep every layer from being pruned to nothing).
4. **Reset** the *surviving* (unpruned) weights back to their **original values from $\theta_0$**
   — not to new random values, and not left at their trained values.
5. Retrain this smaller, masked network from that reset starting point.
6. Repeat steps 2–5 iteratively (prune a further fraction each round) until the desired sparsity
   is reached.

## 3. The Critical Control: Why the Initialization Match Matters
The hypothesis's interesting claim is not just "a sparse subnetwork that trains well exists" — a
much weaker and less surprising statement — but that **the specific original initialization**
matters. The evidence for this comes from a controlled comparison:
- **Winning ticket**: the pruned mask, reset to its values from $\theta_0$ (step 4 above), trained
  from there — typically matches or nearly matches the dense network's accuracy.
- **Random reinitialization control**: the *same* pruned mask (same sparse architecture), but with
  its surviving weights reset to **fresh random values** instead of their original $\theta_0$
  values — typically trains noticeably worse than the winning ticket, at the same sparsity.

Since both networks have the exact same sparse connectivity pattern, the only difference is which
initial *values* the surviving weights started from — isolating initialization, not architecture,
as the active ingredient in the hypothesis's claim.

## 4. Model Compression and Knowledge Distillation (Basics)
Pruning (removing weights) is one of several practically-motivated **model compression**
techniques for getting a smaller, cheaper model to match a larger one's performance; another,
largely orthogonal technique is **knowledge distillation**: training a small "student" network not
only (or not at all) on the original hard labels, but on the softened output probabilities of a
larger, already-trained "teacher" network, which carries more information per example than a
single hard label (the teacher's relative confidence across *all* classes, not just the top one).

## 5. Code: Iterative Magnitude Pruning and a Distillation Sketch
```python
import copy
import torch, torch.nn as nn, torch.nn.functional as F

torch.manual_seed(0)

def make_net():
    return nn.Sequential(nn.Linear(20, 128), nn.ReLU(), nn.Linear(128, 128), nn.ReLU(), nn.Linear(128, 2))

def train(model, X, y, epochs=200, lr=1e-2, mask=None):
    opt = torch.optim.Adam(model.parameters(), lr=lr)
    for _ in range(epochs):
        opt.zero_grad()
        loss = F.cross_entropy(model(X), y)
        loss.backward()
        if mask is not None:
            for p, m in zip(model.parameters(), mask):
                if m is not None:
                    p.grad *= m
        opt.step()
        if mask is not None:
            with torch.no_grad():
                for p, m in zip(model.parameters(), mask):
                    if m is not None:
                        p.mul_(m)
    return model

def accuracy(model, X, y):
    return (model(X).argmax(1) == y).float().mean().item()

X = torch.randn(400, 20)
y = (X[:, 0] + X[:, 1] - X[:, 2] > 0).long()
X_test = torch.randn(1000, 20); y_test = (X_test[:, 0] + X_test[:, 1] - X_test[:, 2] > 0).long()

dense = make_net()
theta0 = copy.deepcopy(dense.state_dict())        # save original initialization
train(dense, X, y)
print("Dense network test accuracy:", round(accuracy(dense, X_test, y_test), 3))

# Build a magnitude-pruning mask (prune 70% of weights, per linear layer, by |weight|).
masks = []
for p in dense.parameters():
    if p.dim() == 2:                              # weight matrices only, not biases
        k = int(0.7 * p.numel())
        thresh = p.abs().flatten().kthvalue(k).values
        masks.append((p.abs() > thresh).float())
    else:
        masks.append(None)

# Winning ticket: reset surviving weights to theta0, retrain under the mask.
winning_ticket = make_net()
winning_ticket.load_state_dict(theta0)
with torch.no_grad():
    for p, m in zip(winning_ticket.parameters(), masks):
        if m is not None:
            p.mul_(m)
train(winning_ticket, X, y, mask=masks)
print("Winning ticket test accuracy:", round(accuracy(winning_ticket, X_test, y_test), 3))

# Random-reinitialization control: same mask, fresh random weights.
random_control = make_net()                       # a fresh random init, NOT theta0
with torch.no_grad():
    for p, m in zip(random_control.parameters(), masks):
        if m is not None:
            p.mul_(m)
train(random_control, X, y, mask=masks)
print("Random-reinit control test accuracy:", round(accuracy(random_control, X_test, y_test), 3))
```
Expect the winning ticket's accuracy to sit close to the dense network's, and the random-
reinitialization control's accuracy to be measurably worse at the same 70% sparsity — the
qualitative signature the Lottery Ticket Hypothesis predicts.

## 6. In-Class Exercise
Explain why resetting pruned weights to *zero permanently* (rather than retraining them from
$\theta_0$ too) is necessary for the mask to actually reduce the network's effective parameter
count, and why the mask itself must stay fixed during the retraining step.
