# Week 2 — Lecture Content: Deep Networks in Practice

*Scope note: initialization, dropout, and regularization were introduced at a basic level in the
prerequisite course. This week deepens each topic with implementation-level detail and adds batch
normalization, which the prerequisite course did not cover.*

## 1. Weight Initialization, Revisited

The prerequisite course established that all-zero (or all-equal) initialization fails by symmetry.
This week derives *how* to choose the scale of random initialization.

**Xavier/Glorot initialization** aims to preserve the variance of activations and gradients across
layers for symmetric activations (tanh/sigmoid). For a layer with $n_{in}$ inputs and $n_{out}$
outputs, weights are drawn so that:

$$
\mathrm{Var}(W) = \frac{2}{n_{in} + n_{out}}
$$

**He initialization** adjusts this for ReLU, which zeroes out roughly half its inputs, so it needs
roughly double the variance to preserve activation scale:

$$
\mathrm{Var}(W) = \frac{2}{n_{in}}
$$

```python
import torch.nn as nn

layer = nn.Linear(256, 256)
nn.init.xavier_uniform_(layer.weight)     # for tanh/sigmoid hidden layers
nn.init.kaiming_normal_(layer.weight, nonlinearity='relu')   # for ReLU hidden layers
nn.init.zeros_(layer.bias)
```

## 2. Batch Normalization

Batch normalization normalizes each activation using the current mini-batch's mean and variance
during training, then rescales with learned parameters $\gamma, \beta$:

$$
\hat{x} = \frac{x - \mu_{\text{batch}}}{\sqrt{\sigma^2_{\text{batch}} + \epsilon}}, \qquad
y = \gamma \hat{x} + \beta
$$

During evaluation, it uses a running average of $\mu$ and $\sigma^2$ accumulated during training,
instead of the current batch's statistics — which is exactly why `model.train()` and
`model.eval()` matter:

```python
import torch.nn as nn

class MLPWithBN(nn.Module):
    def __init__(self, in_features, hidden, out_features):
        super().__init__()
        self.fc1 = nn.Linear(in_features, hidden)
        self.bn1 = nn.BatchNorm1d(hidden)
        self.fc2 = nn.Linear(hidden, out_features)

    def forward(self, x):
        x = torch.relu(self.bn1(self.fc1(x)))
        return self.fc2(x)

model = MLPWithBN(20, 128, 10)
model.train()   # self.bn1 uses batch statistics, and updates its running mean/var
# ... training steps ...
model.eval()    # self.bn1 switches to the accumulated running mean/var
```

Batch normalization stabilizes training by keeping each layer's input distribution from shifting
too much as earlier layers' parameters change, which lets higher learning rates be used reliably.

## 3. Dropout, Revisited

The prerequisite course introduced dropout as randomly zeroing units during training with
inverted-dropout scaling. This week frames it from two complementary angles:

- **Regularization view:** dropout prevents units from relying too heavily on any one other unit,
  discouraging brittle, co-adapted feature detectors.
- **Implicit ensembling view:** each forward pass with a different dropout mask samples a
  different sub-network; training with dropout approximates training an exponential number of
  these sub-networks with shared weights, and inference approximates averaging over them.

```python
dropout = nn.Dropout(p=0.5)
# dropout(x) zeroes ~50% of x's elements and scales the rest by 1/(1-p) during training;
# it is the identity function during model.eval().
```

## 4. Vanishing/Exploding Gradients, Revisited

The prerequisite course introduced this conceptually for deep stacks. Concrete mitigations used in
practice:

| Mitigation | Mechanism |
|---|---|
| Good initialization (Xavier/He) | Starts activations/gradients at a sane scale |
| Batch normalization | Keeps each layer's input distribution stable during training |
| Gradient clipping | Caps gradient norm so a single bad batch cannot destabilize training |
| Skip/residual connections (previewed, Week 4) | Gives gradients a path that bypasses saturating transformations |

```python
import torch.nn.utils as utils

loss.backward()
utils.clip_grad_norm_(model.parameters(), max_norm=1.0)   # cap the gradient norm before stepping
optimizer.step()
```

## 5. In-Class Exercise

A 6-layer MLP's training loss is flat from the very first step. List, in order of cheapest to
check, the diagnostics from this week that could identify the cause (initialization scale, missing
normalization, gradient explosion, learning rate), and explain the order chosen.
