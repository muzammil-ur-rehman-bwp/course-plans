# Week 10 — Lecture Content: Double Descent Revisited Rigorously

## 1. Beyond the Conceptual Introduction
The graduate course introduced double descent conceptually, on the model-size axis: test error
follows the classical U-shape at low capacity, then — contrary to the classical bias-variance
story — can rise again near the point where the model has just enough capacity to fit the training
data exactly (the **interpolation threshold**), before descending a second time as capacity grows
further into the overparameterized regime. Nakkiran et al.'s work establishes this is not an
isolated model-size curiosity: the same qualitative shape, organized around the same concept (an
interpolation threshold), appears along (at least) three distinct axes.

## 2. The Interpolation Threshold as the Organizing Concept
Define the interpolation threshold as the point at which a model's **effective capacity** just
matches what is needed to fit the training set exactly (zero training error). Precisely at this
threshold, the specific interpolating solution found is often poorly conditioned — small,
essentially arbitrary features of the particular training sample get amplified into the fitted
function, because there is just barely enough freedom to fit the data and *no slack left over* to
prefer a "nicer" among the (barely any) fitting solutions. This single idea organizes all three
axes below.

## 3. The Three Axes
- **Model-size double descent (the classical presentation).** For fixed data, sweep model
  capacity (e.g., width). Test error: U-shape, peak near the interpolation threshold (capacity ≈
  amount needed to fit the training set), then descent again as capacity grows well past it —
  increasingly overparameterized models have increasingly many ways to fit the data, and gradient
  descent's implicit bias (Week 4's territory, in spirit) increasingly favors a "nice" one among
  them once there is enough slack to do so.
- **Sample-size double descent.** For a *fixed* model, sweep training-set size $n$. Because the
  interpolation threshold is a relationship between model capacity and $n$, a fixed model has its
  *own* $n$-dependent interpolation threshold — and test error can, perhaps counter-intuitively,
  **transiently worsen as $n$ grows** while approaching that threshold from below, before improving
  again once $n$ grows past it. This directly complicates the folk intuition "more data can never
  hurt," which is only true once one is safely on one side of the relevant threshold or the other.
- **Epoch-wise (training-time) double descent.** For a fixed, (over)parameterized model and
  dataset, sweep training time (epochs). A model can pass through a high-test-error regime
  partway through training — effectively, a point at which the *training dynamics* have produced
  an effective capacity near the interpolation threshold, even though the model's raw parameter
  count never changes — before continuing to train and improving again past that point.

## 4. Theoretical Explanations Proposed
- **Effective-capacity accounts.** These explanations formalize "effective capacity" (not raw
  parameter count) as the quantity that actually matters, and tie the location of the test-error
  peak to wherever effective capacity crosses the amount needed to interpolate the training set —
  correctly predicting that the peak's *location* shifts with any manipulation that changes
  effective capacity (regularization strength, label noise, or, for the epoch-wise axis, training
  time itself acting as an implicit capacity-control knob under certain optimizers).
- **Random-matrix-theoretic connections.** For specific tractable model classes (notably linear
  and random-feature regression), the test-error curve as a function of the ratio of features to
  samples can be computed in closed form using random-matrix theory, and this closed form
  reproduces the qualitative double-descent shape exactly, with a divergence precisely at the
  interpolation threshold (where a key matrix in the closed-form solution becomes near-singular) —
  giving a fully rigorous explanation of double descent's *existence and location* in these
  tractable cases, directly connecting back to this course's own NTK/kernel-regime material
  (Week 2) for networks operating in that regime.
- **Honest status: location versus magnitude.** These explanations robustly account for *where*
  the peak occurs (at the interpolation threshold, however that threshold is defined for the axis
  in question) and *why* a peak should be expected there at all (poor conditioning of
  barely-determined interpolating solutions). They are considerably less complete as explanations
  of the peak's exact *magnitude* and *width* for realistic, finite-width, feature-learning deep
  networks outside the tractable linear/random-feature cases — this remains an active area, not a
  fully closed theoretical question.

## 5. Code: Reproducing Model-Size Double Descent
```python
import torch
import torch.nn as nn
import numpy as np

torch.manual_seed(0)

def make_data(n_train=40, n_test=200, d=20, noise_std=0.5):
    X_train = torch.randn(n_train, d)
    X_test = torch.randn(n_test, d)
    w_true = torch.randn(d)
    y_train = X_train @ w_true + noise_std * torch.randn(n_train)
    y_test = X_test @ w_true + noise_std * torch.randn(n_test)
    return X_train, y_train, X_test, y_test

def train_random_feature_model(X_train, y_train, X_test, width, d, ridge=1e-6, steps=3000, lr=0.05):
    torch.manual_seed(width)
    W = torch.randn(width, d) / d**0.5          # fixed random features (untrained — isolates capacity effect)
    feat_train = torch.relu(X_train @ W.T)
    feat_test = torch.relu(X_test @ W.T)
    beta = torch.zeros(width, requires_grad=True)
    opt = torch.optim.SGD([beta], lr=lr)
    for _ in range(steps):
        opt.zero_grad()
        pred = feat_train @ beta
        loss = ((pred - y_train) ** 2).mean() + ridge * (beta ** 2).sum()
        loss.backward()
        opt.step()
    test_pred = feat_test @ beta.detach()
    return ((test_pred - y_test) ** 2).mean().item()

d, n_train = 20, 40
X_train, y_train, X_test, y_test = make_data(n_train=n_train, d=d)
widths = [5, 10, 20, 30, 40, 50, 60, 100, 200, 400]
for width in widths:
    test_mse = train_random_feature_model(X_train, y_train, X_test, width, d)
    marker = "  <-- near interpolation threshold" if abs(width - n_train) <= 10 else ""
    print(f"width={width:4d}  test MSE={test_mse:8.4f}{marker}")
```
Students should see test MSE rise toward a peak as `width` approaches `n_train` (the interpolation
threshold for this random-feature-regression setup) and fall again as `width` grows well past it —
the double-descent shape, reproduced at small scale with a fully tractable, random-feature model.

## 6. In-Class Exercise
Using §3's sample-size-axis description, predict (without running code) what should happen to test
error if, holding model width fixed at a value *just below* the interpolation threshold for
`n_train=40`, you instead sweep `n_train` upward from a small value toward and past 40. Sketch the
predicted curve's shape and label where its peak should occur.
