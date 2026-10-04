# Week 14 — Lecture Content: Information-Theoretic Perspectives

## 1. The Information Bottleneck Idea
The information bottleneck principle, originating in Tishby and collaborators' work on
information-theoretic learning and later applied specifically to deep network training, frames a
hidden representation $T$ (e.g., a layer's activations) of an input $X$ used to predict a label
$Y$ as trading off two mutual-information quantities:
$$
\min_{p(t\mid x)}\ \ I(X;T) \;-\; \beta\, I(T;Y)
$$
- $I(T;Y)$ (the **fitting** term): how much information $T$ retains about the label — larger is
  better for prediction.
- $I(X;T)$ (the **compression** term): how much information $T$ retains about the raw input —
  the principle penalizes retaining *more* of $X$ than is needed to predict $Y$, favoring a
  representation that discards input information irrelevant to the task.
$\beta$ controls the tradeoff. Applied to a trained deep network layer-by-layer, the conjecture is
that training proceeds through (at least) two conceptually distinct phases: an early **fitting**
phase where $I(T;Y)$ rises as the representation becomes predictive, and (in some reported
analyses) a later **compression** phase where $I(X;T)$ decreases even as $I(T;Y)$ stays high — the
network discarding input information not needed for the task, after it has already learned to
predict well.

## 2. The Evidence and the Dispute
The reported "compression phase" is based on **estimating mutual information** between continuous,
high-dimensional hidden activations and the input/output — a statistically hard problem, commonly
approximated in this line of work with binning-based estimators. Subsequent published critiques
(associated with researchers including Saxe and collaborators) raised substantive concerns:
- The specific compression behavior reported depended strongly on the choice of **activation
  function** — observed clearly with saturating activations (e.g., tanh) but much less clearly,
  or not at all, with non-saturating activations such as ReLU, suggesting the phenomenon may be
  partly an artifact of activation saturation rather than a general property of deep learning.
- Binning-based mutual information estimators can behave in ways that do not straightforwardly
  reflect the *information-theoretic* quantity they are meant to estimate for a deterministic,
  continuous map (a feedforward layer, for fixed weights, is a deterministic function of its
  input, which raises subtleties for how its mutual information with the input should even be
  defined and estimated in practice).

This course presents the information bottleneck idea as a **conceptually valuable lens** — it
gives a principled vocabulary for asking what a layer is keeping versus discarding — while being
explicit that whether it is a *correct causal account* of why deep networks generalize is an
**unsettled, actively disputed research question**, not a result to take as given.

## 3. How to Hold This Honestly (Framing Practice)
When presenting (or writing about, for the capstone) a debated research idea like this one, state:
(1) the precise claim being made, (2) the strength and nature of the evidence for it, (3) the
specific, named critiques, and (4) what would need to be true for the claim to be considered
resolved either way. This is practiced concretely here and reinforced as a general skill in Week
15's research-methods unit.

## 4. Code: A Toy Binned Mutual-Information Estimate Across Training
```python
import numpy as np
import torch, torch.nn as nn

def binned_mi_estimate(h, y, n_bins=10):
    """A crude binned mutual-information estimate between a continuous scalar h and a binary y.
    For illustration only -- see Section 2 for why such estimators are contested."""
    bins = np.linspace(h.min() - 1e-9, h.max() + 1e-9, n_bins + 1)
    h_binned = np.digitize(h, bins) - 1
    mi = 0.0
    n = len(h)
    p_y = np.array([np.mean(y == c) for c in [0, 1]])
    for b in range(n_bins):
        mask = h_binned == b
        p_b = mask.mean()
        if p_b == 0:
            continue
        for c in [0, 1]:
            p_bc = np.mean(mask & (y == c))
            if p_bc > 0:
                mi += p_bc * np.log(p_bc / (p_b * p_y[c]))
    return mi

torch.manual_seed(0)
X = torch.randn(300, 10)
y = (X[:, 0] + X[:, 1] > 0).long()
model = nn.Sequential(nn.Linear(10, 8), nn.Tanh(), nn.Linear(8, 2))
opt = torch.optim.Adam(model.parameters(), lr=1e-2)

for epoch in range(0, 201, 50):
    hidden = model[1](model[0](X)).detach().numpy()[:, 0]     # first hidden unit's activation
    mi_est = binned_mi_estimate(hidden, y.numpy())
    print(f"epoch {epoch:3d}  I(T_1;Y) estimate (nats): {mi_est:.4f}")
    for _ in range(50):
        opt.zero_grad()
        loss = nn.functional.cross_entropy(model(X), y)
        loss.backward(); opt.step()
```
This toy estimate is deliberately crude (one hidden unit, coarse binning) — its purpose is to let
students *feel* the mechanics of a binned MI estimate and immediately see, by varying `n_bins`,
how sensitive the resulting numbers are to an essentially arbitrary estimator choice — a concrete,
hands-on route into Section 2's critique.

## 5. In-Class Exercise
Re-run the estimate with `n_bins=3` and `n_bins=50` and report how much the computed $I(T_1;Y)$
value changes — connecting the result directly to the published critique that binned MI estimates
are sensitive to estimator choices in ways that complicate strong claims built on them.
