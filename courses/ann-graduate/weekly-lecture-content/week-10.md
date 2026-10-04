# Week 10 — Lecture Content: Generalization Theory II

## 1. Rademacher Complexity
**(Empirical) Rademacher complexity** measures a hypothesis class's capacity to fit *random*
noise on a *specific* sample $S=\{x_1,\dots,x_m\}$, rather than worst-case over all possible point
sets (as VC dimension does):
$$
\widehat{\mathcal{R}}_S(H) = \mathbb{E}_{\sigma}\left[\sup_{h\in H}\ \frac{1}{m}\sum_{i=1}^m \sigma_i h(x_i)\right]
$$
where $\sigma_1,\dots,\sigma_m$ are independent, uniform $\{-1,+1\}$ (Rademacher) random
variables. Intuitively: draw a completely random labeling $\sigma$, and ask how well the *best*
hypothesis in $H$ can correlate with that pure noise, on these particular points, averaged over
many random labelings. A class with large Rademacher complexity can fit noise well on this data —
a sign it may also fit real labels partly through memorizing noise-like artifacts rather than
genuine signal. Because $\widehat{\mathcal{R}}_S(H)$ is evaluated on the *actual* data distribution
seen in $S$, it can be dramatically smaller than a distribution-free VC-dimension bound for the
same class, and generalization bounds built from it,
$$
\mathrm{error}_D(h) \;\leq\; \widehat{\mathrm{error}}(h) + 2\widehat{\mathcal{R}}_S(H) + O\!\left(\sqrt{\tfrac{\log(1/\delta)}{m}}\right),
$$
can be correspondingly tighter and more informative than the Week 9 VC bound, in favorable cases.

## 2. Margin-Based Generalization Arguments
For a classifier that doesn't just separate the training data but does so with a **large margin**
$\gamma$ (distance from the decision boundary to the nearest training point, suitably normalized),
generalization bounds can be stated in terms of $\gamma$ instead of raw capacity: informally, the
generalization gap scales like (a complexity measure of $H$) $/\gamma$, so a larger margin shrinks
the *effective* complexity the bound charges, even for a class with large raw VC dimension or
Rademacher complexity. This matters for neural networks because, even though the raw hypothesis
class (all networks of a given architecture) has enormous nominal capacity, the specific solution
gradient descent finds often has a comparatively large margin on the training data relative to its
raw capacity — margin-based arguments are one way of explaining why *that particular* solution
generalizes better than a capacity-only argument would suggest for *some* solution in the class.

## 3. The Random-Label-Fitting Phenomenon
A now-classic experiment, widely discussed in the generalization literature: take a standard image
classification architecture and training procedure, but **replace every training label with an
independently drawn random label**. The network is still able to drive training error to (or very
near) zero — confirming the network's raw capacity really is enormous, consistent with Week 9's
VC-dimension argument. The same architecture, trained **normally** (on the real, non-randomized
labels) with the same optimizer, achieves low training error **and** low test error. Two separate
facts are easy to conflate here and must be kept distinct:
- *Capacity* (can the class fit arbitrary labels at all?) — yes, enormous, as the random-label
  experiment shows.
- *What gradient descent actually finds, on real data* — empirically, something that generalizes
  well, even though the class as a whole could also represent wildly non-generalizing solutions
  (e.g., the one that memorized random labels).

This decisively rules out "the network just doesn't have enough capacity to overfit" as an
explanation for good generalization, and places the explanatory burden instead on the **implicit
bias of the training procedure** (which specific low-training-error solution gradient descent is
drawn to, out of the astronomically many that exist) — a theme continued in the NTK and Lottery
Ticket units (Weeks 12–13).

## 4. Code: Rademacher-Complexity Estimate and Random-Label Fitting
```python
import numpy as np
import torch, torch.nn as nn

# --- Part A: empirical Rademacher complexity estimate for a small linear class
def rademacher_estimate(X, n_sigma_draws=200, seed=0):
    rng = np.random.default_rng(seed)
    m, d = X.shape
    vals = []
    for _ in range(n_sigma_draws):
        sigma = rng.choice([-1, 1], size=m)
        # sup over unit-norm linear hypotheses h(x) = w.x, ||w||<=1: achieved by w = Xt.sigma / ||Xt.sigma||
        corr = X.T @ sigma
        sup_val = np.linalg.norm(corr) / m          # since sup_{||w||<=1} w.(X^T sigma)/m = ||X^T sigma||/m
        vals.append(sup_val)
    return np.mean(vals)

rng = np.random.default_rng(0)
X_small = rng.normal(size=(20, 5))
print("Empirical Rademacher complexity estimate:", round(rademacher_estimate(X_small), 4))

# --- Part B: fitting random labels with a small MLP
def make_mlp():
    return nn.Sequential(nn.Linear(20, 64), nn.ReLU(), nn.Linear(64, 64), nn.ReLU(), nn.Linear(64, 1))

torch.manual_seed(0)
X_train = torch.randn(200, 20)
y_true = (X_train[:, 0] + X_train[:, 1] > 0).float().unsqueeze(1)
y_random = torch.randint(0, 2, (200, 1)).float()
X_test = torch.randn(1000, 20)
y_test_true = (X_test[:, 0] + X_test[:, 1] > 0).float().unsqueeze(1)

for label_name, y_train in [("real labels", y_true), ("random labels", y_random)]:
    model = make_mlp()
    opt = torch.optim.Adam(model.parameters(), lr=1e-2)
    loss_fn = nn.BCEWithLogitsLoss()
    for _ in range(500):
        opt.zero_grad()
        loss = loss_fn(model(X_train), y_train)
        loss.backward(); opt.step()
    train_acc = ((model(X_train) > 0).float() == y_train).float().mean().item()
    test_acc = ((model(X_test) > 0).float() == y_test_true).float().mean().item()
    print(f"{label_name:14s} train acc={train_acc:.3f}  test acc (vs. true concept)={test_acc:.3f}")
```
Expect both runs to reach high (often ~100%) training accuracy — confirming capacity — while only
the real-labels run achieves test accuracy well above chance; the random-labels run's test
accuracy against the *true* underlying concept should sit near chance ($\approx 0.5$), since it
learned to memorize noise rather than the real decision rule.

## 5. In-Class Exercise
Explain, in your own words, why "this network can fit random labels" and "this network, trained
normally, overfits real data" are *not* the same claim, using Part B's two accuracy numbers as a
concrete illustration.
