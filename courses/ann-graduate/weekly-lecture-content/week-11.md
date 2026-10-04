# Week 11 — Lecture Content: Expressivity and Depth

*Scope reminder: this week studies two architectural ideas — skip connections and attention —
strictly through a one-week theory lens (optimization and expressivity). Their architectures
(ResNets, Transformers) are covered in depth in the sibling Deep Learning, Graduate course, not
here.*

## 1. The Optimization-Theory Argument for Skip Connections
A plain feedforward block computes $y = F(x)$ for some learned function $F$ (an affine map plus
activation). A **residual/skip** block instead computes
$$
y = x + F(x)
$$
If $F$ is initialized so that its output starts near $\mathbf{0}$ (a common and deliberate
practice for residual blocks), then at initialization $y \approx x$: the block implements an
(approximate) **identity map** from the very first forward pass. The local Jacobian of a residual
block is
$$
\frac{\partial y}{\partial x} = I + \frac{\partial F}{\partial x}
$$
compared to a plain block's Jacobian $\partial F/\partial x$ alone. Composing $L$ plain blocks'
Jacobians (needed to backpropagate a gradient through $L$ layers) is a product of $L$ matrices,
each of which can have norm less than $1$ — the product's norm can shrink geometrically with $L$,
the familiar **vanishing gradient** problem for very deep plain networks. Composing $L$ residual
blocks' Jacobians, each of the form $I + \partial F/\partial x$ with $\partial F/\partial x$ small
near identity-initialization, keeps every factor close to $I$, so the product stays close to $I$
as well — gradients can flow through many stacked residual blocks largely undiminished. This is
the core **optimization-theory** (not purely "architecture") argument for why skip connections
make very deep networks trainable at all: they change the *local geometry near initialization*
from "many small multiplicative factors" to "many near-identity factors," directly addressing the
vanishing-gradient mechanism.

## 2. The Expressivity Argument for Attention (Brief)
In a purely sequential (e.g., strictly layer-by-layer, position-by-position) or local
(convolutional) computation, information from position $i$ must pass through $O(\text{distance})$
intermediate computation steps to influence position $j$ far away — each step is one more
opportunity for the signal (or its gradient) to degrade. An attention layer instead computes, for
every position, a weighted combination over *all* positions in one layer:
$$
y_i = \sum_j \alpha_{ij}\, v_j, \qquad \alpha_{ij} = \mathrm{softmax}_j\big(\text{score}(q_i,k_j)\big)
$$
This gives **every pair of positions a direct, $O(1)$-length path** for information to flow in a
single layer, regardless of how far apart they are — a structurally different expressivity profile
from a mechanism whose path length between distant positions grows with sequence length or with
network depth. This is, again, only the expressivity-theory headline; the attention mechanism's
full architecture (multi-head attention, the Transformer block, positional encoding, etc.) is
covered in the Deep Learning, Graduate course.

## 3. Code: Gradient Flow, Plain vs. Skip-Connected Depth
```python
import torch, torch.nn as nn

def make_plain_net(depth, width=64):
    layers = []
    for _ in range(depth):
        layers += [nn.Linear(width, width), nn.Tanh()]
    return nn.Sequential(*layers)

class ResidualBlock(nn.Module):
    def __init__(self, width):
        super().__init__()
        self.fc = nn.Linear(width, width)
        nn.init.zeros_(self.fc.weight)      # start the residual branch at (near) zero
        nn.init.zeros_(self.fc.bias)

    def forward(self, x):
        return x + torch.tanh(self.fc(x))

def make_residual_net(depth, width=64):
    return nn.Sequential(*[ResidualBlock(width) for _ in range(depth)])

def grad_norm_at_input(net, width=64, depth=40):
    x = torch.randn(1, width, requires_grad=True)
    y = net(x)
    loss = y.sum()
    loss.backward()
    return x.grad.norm().item()

depth = 40
plain_grad = grad_norm_at_input(make_plain_net(depth), depth=depth)
residual_grad = grad_norm_at_input(make_residual_net(depth), depth=depth)
print(f"Plain net   ({depth} layers), input grad norm: {plain_grad:.3e}")
print(f"Residual net({depth} layers), input grad norm: {residual_grad:.3e}")
```
Expect the plain network's input gradient norm to be dramatically smaller (often many orders of
magnitude) than the residual network's at this depth, directly illustrating Section 1's argument.

## 4. In-Class Exercise
Explain why initializing the residual branch $F$'s *last* layer to exactly zero (rather than
small-random) makes the identity-at-initialization argument exact rather than merely approximate,
and why this doesn't prevent $F$ from eventually learning something useful once training begins.
