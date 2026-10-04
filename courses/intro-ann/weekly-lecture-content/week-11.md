# Week 11 — Lecture Content: Regularization

## 1. Overfitting in Neural Networks
A network with enough capacity (enough units/layers) can drive training loss arbitrarily close
to zero by effectively memorizing the training set, including its noise — at the cost of
generalizing poorly to new data. **Regularization** techniques constrain a network's effective
capacity or otherwise discourage memorization, trading a little training performance for better
validation/test performance.

## 2. L1 and L2 Weight Regularization
Both add a penalty term to the loss based on the weights' magnitude:

$$
L_{\text{L2}} = L_{\text{data}} + \frac{\lambda}{2}\sum_l \lVert W^{(l)} \rVert_F^2, \qquad
L_{\text{L1}} = L_{\text{data}} + \lambda \sum_l \lVert W^{(l)} \rVert_1
$$

where $\lambda \ge 0$ controls regularization strength. Differentiating the L2 penalty term with
respect to $W^{(l)}$ adds $\lambda W^{(l)}$ to the gradient, so the parameter update becomes:

$$
W^{(l)} \leftarrow W^{(l)} - \eta\left(\frac{\partial L_{\text{data}}}{\partial W^{(l)}} + \lambda W^{(l)}\right) = (1-\eta\lambda)W^{(l)} - \eta\frac{\partial L_{\text{data}}}{\partial W^{(l)}}
$$

The $(1-\eta\lambda)$ factor shrinks every weight slightly on every step regardless of the data
gradient — this is why L2 regularization is also called **weight decay**. L1's penalty gradient
is $\lambda\,\text{sign}(W^{(l)})$, a constant pull toward zero regardless of magnitude, which
tends to drive many weights to *exactly* zero (a sparsifying effect), unlike L2's proportional
shrinkage.

```python
import numpy as np

def l2_penalty_grad(W, lam):
    return lam * W

def l1_penalty_grad(W, lam):
    return lam * np.sign(W)
```

## 3. Dropout
During training, dropout randomly deactivates each hidden unit with probability $p$ (independently,
per unit, per forward pass), forcing the network to not rely too heavily on any single unit (each
unit must work well regardless of which others happen to be present). At test time, no units are
dropped; **inverted dropout** scales activations during training by $1/(1-p)$ so that the expected
output magnitude matches between training and test time without needing to rescale at test time:

```python
def dropout_forward(a, p, training, rng):
    if not training or p == 0:
        return a, None
    mask = (rng.random(a.shape) > p).astype(a.dtype)
    a_dropped = a * mask / (1 - p)     # inverted dropout: scale up surviving units
    return a_dropped, mask

def dropout_backward(da, mask, p):
    if mask is None:
        return da
    return da * mask / (1 - p)         # gradient only flows through surviving units
```
Typical $p$ values are 0.2–0.5 for hidden layers. Dropout is applied only during training; the
`training` flag must be set to `False` for evaluation/inference.

## 4. Early Stopping
Early stopping monitors validation loss during training and halts (or restores the best-seen
weights) once validation loss stops improving for a set number of epochs ("patience"), even
though training loss may still be decreasing. This directly exploits the diagnostic pattern from
Week 8/15: a widening gap between training and validation loss is itself the overfitting signal.

```python
best_val_loss = float("inf")
patience, patience_counter = 5, 0
best_weights = None

for epoch in range(max_epochs):
    train_one_epoch(...)
    val_loss = compute_validation_loss(...)
    if val_loss < best_val_loss:
        best_val_loss = val_loss
        best_weights = copy_weights(...)
        patience_counter = 0
    else:
        patience_counter += 1
        if patience_counter >= patience:
            break    # stop training; restore best_weights
```

## 5. In-Class Exercise
Given a training/validation loss curve where training loss decreases steadily but validation
loss starts increasing after epoch 30, identify the epoch at which early stopping (with patience
5) would have halted training, and propose which of L2, dropout, or both you would add, with a
one-sentence justification.
