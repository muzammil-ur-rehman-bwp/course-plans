# Week 13 — Lecture Content: Optimization and Regularization for Deep Nets, Revisited

## 1. Weight Decay vs. L2 Regularization Under Adam

The prerequisite course showed that adding an L2 penalty $\frac{\lambda}{2}\|\theta\|^2$ to the
loss is equivalent, under plain SGD, to directly shrinking ("decaying") the weights by a factor
each step — the two views coincide because SGD's update is a direct, unscaled step along the
gradient.

Adam does not take an unscaled step: it divides the gradient by a per-parameter running estimate
of its magnitude (Section 8.3/8.5 territory from the prerequisite course's optimizer treatment).
If an L2 penalty is added to the loss *before* computing Adam's adaptive update, the penalty's
gradient contribution gets rescaled by that same per-parameter adaptive factor — so parameters
with large, noisy gradients get *less* effective weight decay, and parameters with small gradients
get *more*, which is not the uniform shrinkage weight decay is meant to provide.

**AdamW** fixes this by decoupling weight decay from the gradient-based update: it applies Adam's
usual adaptive step, then separately subtracts $\lambda \theta$ directly from the weights, so decay
strength no longer depends on each parameter's gradient scale.

```python
import torch.optim as optim

# L2 penalty folded into the gradient (the "naive" approach under Adam):
optimizer_l2 = optim.Adam(model.parameters(), lr=1e-3, weight_decay=1e-4)  # torch.optim.Adam's
                                                                             # weight_decay IS this L2-in-gradient form

# Decoupled weight decay:
optimizer_adamw = optim.AdamW(model.parameters(), lr=1e-3, weight_decay=1e-4)
```

## 2. Label Smoothing

Standard cross-entropy with a one-hot target pushes the model toward assigning probability 1 to
the correct class and 0 to all others — a target the model can approach but never exactly reach,
which can encourage overconfident, poorly calibrated predictions. Label smoothing softens the
target: for $K$ classes and smoothing factor $\epsilon$, the target for the correct class becomes
$1 - \epsilon$ and each incorrect class gets $\epsilon / (K - 1)$ instead of exactly 0.

```python
import torch.nn as nn

criterion = nn.CrossEntropyLoss(label_smoothing=0.1)   # built-in support
```

## 3. Mixed-Precision Training (Conceptual)

Training in float16/bfloat16 instead of float32 roughly halves memory use and can substantially
speed up matrix multiplications on modern GPU hardware. The risk is numerical: float16 has a
narrower dynamic range, so small gradient values can underflow to zero. The standard recipe keeps
a float32 "master" copy of the weights, computes the forward/backward pass in float16, and scales
the loss up before the backward pass (then scales gradients back down before the optimizer step)
to keep small gradients representable — this is **loss scaling**.

```python
import torch

scaler = torch.cuda.amp.GradScaler()

for xb, yb in train_loader:
    optimizer.zero_grad()
    with torch.cuda.amp.autocast():       # forward pass runs in float16/bfloat16 where safe
        loss = criterion(model(xb), yb)
    scaler.scale(loss).backward()         # loss scaling applied automatically
    scaler.step(optimizer)
    scaler.update()
```

## 4. Large-Batch Training Considerations

Increasing batch size reduces gradient noise, which (within limits) allows a proportionally larger
learning rate — a common heuristic is **linear LR scaling**: if batch size doubles, scale the
learning rate by roughly the same factor. This only works reliably combined with the warmup
introduced in Week 5: a large learning rate applied immediately (rather than ramped up) is far
more likely to destabilize early training, and that risk grows with batch size.

```python
base_lr, base_batch_size = 0.1, 256
new_batch_size = 1024
scaled_lr = base_lr * (new_batch_size / base_batch_size)   # linear scaling rule
```

## 5. In-Class Exercise

Explain, using the per-parameter adaptive scaling from Section 1, why `Adam(weight_decay=...)`
and `AdamW(weight_decay=...)` can produce different trained models even with the same numeric
`weight_decay` value.
