# Week 10 — Lecture Content: Neural Architecture Search

## 1. The NAS Problem, Decomposed
Neural Architecture Search (NAS) automates the design of a network's architecture. Any NAS method
is fully specified by three components:
- **Search space.** The set of candidate architectures — e.g., a *cell-based* space (choose, for
  a small repeating "cell," which operations to apply and how to connect them, then stack the cell
  many times) or a *macro* space (choose per-layer width/depth/operation directly).
- **Search strategy.** The algorithm that proposes which candidate(s) to evaluate next — random
  search, RL-based, evolutionary, or differentiable (gradient-based), covered below.
- **Performance estimation strategy.** How a candidate's quality is scored. Training every
  candidate to full convergence is usually far too expensive, so practical NAS methods use cheaper
  proxies: training for only a few epochs, training on a subset of data, or (most importantly for
  the efficiency story below) sharing weights across candidates so a "fully trained" one-shot model
  can estimate many candidates' quality without training each from scratch.

## 2. RL-Based Search (Conceptual)
Treat architecture generation as a sequential decision process: a **controller** (e.g., a small
RNN) samples a sequence of discrete choices (one per architectural decision — layer type, kernel
size, connection pattern, etc.), which together fully specify a candidate architecture. The
candidate is then trained (or proxy-evaluated) and its validation performance is used as a
**reward**. The controller's parameters are updated via a policy-gradient method — **this is a
direct application of the graduate course's REINFORCE estimator, referenced here, not
re-derived**: the controller is the policy, the sampled architecture is the "action sequence," and
validation accuracy (or a function of it) is the return. Early RL-based NAS, training every
sampled candidate from scratch to get a reward signal, is extremely expensive — this motivates
Section 5.

## 3. Evolutionary Search (Conceptual)
Maintain a **population** of candidate architectures (encoded, e.g., as a fixed-length vector of
discrete choices). At each generation: **mutate** some candidates (apply small random edits to
their encoding — change one layer's width, swap one operation), evaluate the resulting
population's performance (again, typically via a cheap proxy), and **select** the
better-performing candidates to survive/reproduce into the next generation (e.g., simple
truncation selection: keep the top fraction, discard the rest, and mutate survivors to refill the
population). No gradient information is used at all — evolutionary search only ever compares
candidates' scores.

```python
import random

def mutate(arch, search_space, rate=0.2):
    """arch: list of indices into each position's candidate-op list in search_space."""
    new_arch = list(arch)
    for i in range(len(new_arch)):
        if random.random() < rate:
            new_arch[i] = random.randrange(len(search_space[i]))
    return new_arch

def evolutionary_search(search_space, fitness_fn, pop_size=20, generations=15, keep_frac=0.3):
    population = [[random.randrange(len(choices)) for choices in search_space]
                  for _ in range(pop_size)]
    for gen in range(generations):
        scored = sorted(population, key=fitness_fn, reverse=True)
        survivors = scored[: int(pop_size * keep_frac)]
        population = list(survivors)
        while len(population) < pop_size:
            parent = random.choice(survivors)
            population.append(mutate(parent, search_space))
    return max(population, key=fitness_fn)
```

## 4. Differentiable NAS (Conceptual, DARTS-Style)
Rather than discretely choosing one operation at each position in the architecture, **relax** the
choice into a continuous mixture: at a given edge/position with candidate operations
$o_1,\dots,o_m$, define
```
ō(x) = Σᵢ softmax(α)ᵢ · oᵢ(x)
```
where $\alpha \in \mathbb R^m$ is a learnable vector of **architecture weights** (logits over
operation choice). The mixed operation $\bar o$ is now a fully differentiable function of both the
ordinary network weights *and* $\alpha$, so both can be optimized jointly by gradient descent
(typically alternating or bi-level optimization between network weights and architecture weights).
After training, the final discrete architecture is obtained by **discretizing**: at each position,
keep only the operation with the largest $\mathrm{softmax}(\alpha)$ weight (an arg-max), discarding
the continuous mixture. This replaces an outer discrete-search loop entirely with ordinary gradient
descent — by far the cheapest search strategy of the three, at the cost of needing a search space
expressible as a differentiable mixture.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class MixedOp(nn.Module):
    """A toy differentiable relaxation over 3 candidate operations at one point in a network."""
    def __init__(self, dim):
        super().__init__()
        self.ops = nn.ModuleList([
            nn.Linear(dim, dim),                       # candidate op 1: linear
            nn.Sequential(nn.Linear(dim, dim), nn.ReLU()),  # candidate op 2: linear + ReLU
            nn.Identity(),                              # candidate op 3: skip/no-op
        ])
        self.alpha = nn.Parameter(torch.zeros(len(self.ops)))   # architecture weights

    def forward(self, x):
        weights = F.softmax(self.alpha, dim=0)
        return sum(w * op(x) for w, op in zip(weights, self.ops))

    def discretize(self):
        return int(torch.argmax(self.alpha).item())
```

## 5. Why NAS's Own Search Cost Is a Central Practical Constraint
Early RL-based NAS (fully training each sampled candidate from scratch to compute its reward)
required on the order of **thousands of GPU-days** to find a single competitive architecture —
larger than the cost of training the final architecture many times over. This is not a footnote:
it is the single biggest practical obstacle NAS research has had to solve, and essentially every
subsequent development is a direct response to it, not an independent line of improvement:
**weight sharing** (train one large "one-shot" network once, and estimate any candidate
sub-architecture's quality by evaluating the corresponding sub-network within it, avoiding
training each candidate from scratch) and **differentiable relaxation** (Section 4, which replaces
the discrete outer search loop with ordinary joint gradient descent) both exist specifically to
cut search cost by orders of magnitude relative to naive RL-based or evolutionary search with
full-training performance estimation.

## 6. The Efficiency-Accuracy Tradeoff
Every choice in this week's material trades search cost against estimate fidelity: a full-training
performance estimate is accurate but prohibitively slow; a cheap proxy (few-epoch training, weight
sharing, differentiable relaxation) is fast but may rank candidates differently than full training
would — and specifically, weight-sharing/differentiable methods are known to sometimes produce
architecture rankings that correlate imperfectly with each candidate's true, independently-trained
performance, a genuine, actively-studied limitation of the efficiency gains this section describes
and not merely a hypothetical concern.

## 7. In-Class/Lab Exercise
Define a toy search space (e.g., per-layer width choices $\in\{16,32,64\}$ and activation choices
$\in\{\mathrm{ReLU},\mathrm{GELU}\}$ for a 3-layer MLP) and a cheap fitness function (validation
accuracy after a fixed, small number of training steps on a toy dataset). Run
`evolutionary_search` and track best-found fitness across generations. Separately, build a
3-layer MLP with a `MixedOp` at one position and train it jointly with the rest of the network's
weights on the same toy task; plot how $\mathrm{softmax}(\alpha)$ evolves over training and
confirm it concentrates on one operation. Compare each method's total compute spent searching
against the (much larger) hypothetical cost of fully training every candidate in the search space
from scratch.
