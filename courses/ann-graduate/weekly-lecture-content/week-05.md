# Week 5 — Lecture Content: Normalization Theory

## 1. Batch Normalization — Forward Pass
Good initialization (Week 4) preserves activation variance **at $t=0$**. As training proceeds,
each layer's input distribution keeps shifting as earlier layers' weights update — Batch
Normalization (BatchNorm) re-normalizes activations at *every* forward pass, not just at
initialization. For a mini-batch $\{x_1,\dots,x_m\}$ of a single feature (applied independently
per feature/channel):
$$
\mu_B = \frac{1}{m}\sum_{i=1}^m x_i, \qquad \sigma_B^2 = \frac{1}{m}\sum_{i=1}^m (x_i-\mu_B)^2
$$
$$
\hat x_i = \frac{x_i - \mu_B}{\sqrt{\sigma_B^2+\epsilon}}, \qquad y_i = \gamma\,\hat x_i + \beta
$$
where $\gamma,\beta$ are learned per-feature parameters that let the network recover a non-unit
variance or non-zero mean if that is what minimizes the loss, and $\epsilon$ prevents division by
zero. **At evaluation time**, batch statistics are unavailable or undesirable (a single test
example has no "batch"), so BatchNorm uses a running average of $\mu_B,\sigma_B^2$ accumulated
during training — a common and serious implementation bug is using the *current* batch's
statistics at evaluation time instead.

## 2. Batch Normalization — Backward Pass
Let $\bar y_i \equiv \partial L/\partial y_i$ be given (from the next layer). Then:
$$
\bar\gamma = \sum_i \bar y_i\,\hat x_i, \qquad \bar\beta = \sum_i \bar y_i, \qquad \bar{\hat x}_i = \bar y_i\,\gamma
$$
Backpropagating through the normalization requires the chain rule through **both** $\mu_B$ and
$\sigma_B^2$, since every $\hat x_i$ depends on both batch statistics, which in turn depend on
every $x_i$ in the batch:
$$
\overline{\sigma_B^2} = \sum_i \bar{\hat x}_i\,(x_i-\mu_B)\cdot\Big(-\tfrac{1}{2}\Big)(\sigma_B^2+\epsilon)^{-3/2}
$$
$$
\bar\mu_B = \sum_i \bar{\hat x}_i\cdot\Big(-\tfrac{1}{\sqrt{\sigma_B^2+\epsilon}}\Big) + \overline{\sigma_B^2}\cdot\frac{1}{m}\sum_i -2(x_i-\mu_B)
$$
$$
\bar x_i = \bar{\hat x}_i\cdot\frac{1}{\sqrt{\sigma_B^2+\epsilon}} + \overline{\sigma_B^2}\cdot\frac{2(x_i-\mu_B)}{m} + \bar\mu_B\cdot\frac{1}{m}
$$
(The second term of $\bar\mu_B$ vanishes identically because $\sum_i(x_i-\mu_B)=0$ by definition
of the mean, but a correct implementation computes it exactly as above rather than dropping terms
by hand, to stay robust to refactoring.)

## 3. Why Does It Work? Two Competing Explanations
- **Internal covariate shift (the original 2015 explanation).** The claim: BatchNorm helps
  because it reduces the change in each layer's input distribution ("internal covariate shift")
  caused by earlier layers' parameter updates, which supposedly destabilizes training.
- **Loss-landscape smoothing (the better-supported account).** Later analysis showed BatchNorm's
  practical benefit is better explained by its effect on the **optimization landscape** itself:
  normalizing activations provably makes the loss (and its gradients) with respect to the
  normalized activations have a smaller Lipschitz constant and smaller gradient-prediction error
  along the descent direction — in plain terms, it makes the loss surface **smoother and more
  predictable** near the current point, which lets gradient descent safely take larger steps.
  Direct experiments that artificially inject the kind of distributional shift BatchNorm supposedly
  removes, while keeping networks trained normally otherwise, find such injected shift does *not*
  by itself hurt training — undermining the internal-covariate-shift account as the primary
  mechanism. This course presents the loss-landscape-smoothing account as the better-supported
  explanation, while noting the debate is part of the historical record, not fully closed in every
  detail.

## 4. Layer Normalization
Layer Normalization (LayerNorm) normalizes across the **feature dimension for each example
independently**, rather than across the batch dimension for each feature:
$$
\mu_i = \frac{1}{d}\sum_{k=1}^d x_{ik}, \qquad \sigma_i^2 = \frac{1}{d}\sum_{k=1}^d (x_{ik}-\mu_i)^2, \qquad \hat x_{ik} = \frac{x_{ik}-\mu_i}{\sqrt{\sigma_i^2+\epsilon}}
$$
Because this computation needs only example $i$'s own features, it has **no dependence on batch
size or other examples in the batch** — it is identical at training and evaluation time, with no
running-statistics bookkeeping needed. This makes LayerNorm the standard choice for sequence
models (where "batch" composition of variable-length sequences is awkward) and for settings with
very small or batch-size-1 inference, where BatchNorm's batch statistics become unreliable or
undefined.

## 5. Code: BatchNorm From Scratch
```python
import numpy as np

class BatchNorm1D:
    def __init__(self, dim, momentum=0.9, eps=1e-5):
        self.gamma, self.beta = np.ones(dim), np.zeros(dim)
        self.running_mean, self.running_var = np.zeros(dim), np.ones(dim)
        self.momentum, self.eps = momentum, eps

    def forward(self, x, training=True):
        if training:
            mu, var = x.mean(axis=0), x.var(axis=0)
            self.running_mean = self.momentum * self.running_mean + (1 - self.momentum) * mu
            self.running_var = self.momentum * self.running_var + (1 - self.momentum) * var
        else:
            mu, var = self.running_mean, self.running_var
        self.x_centered = x - mu
        self.std_inv = 1.0 / np.sqrt(var + self.eps)
        self.x_hat = self.x_centered * self.std_inv
        return self.gamma * self.x_hat + self.beta

    def backward(self, dy):
        m = dy.shape[0]
        dgamma = (dy * self.x_hat).sum(axis=0)
        dbeta = dy.sum(axis=0)
        dx_hat = dy * self.gamma
        dvar = (dx_hat * self.x_centered * -0.5 * self.std_inv ** 3).sum(axis=0)
        dmu = (dx_hat * -self.std_inv).sum(axis=0) + dvar * (-2.0 * self.x_centered).mean(axis=0)
        dx = dx_hat * self.std_inv + dvar * 2.0 * self.x_centered / m + dmu / m
        return dx, dgamma, dbeta

# Gradient check against a scalar loss L = 0.5 * sum(y**2)
rng = np.random.default_rng(0)
x = rng.normal(size=(8, 4))
bn = BatchNorm1D(4)
y = bn.forward(x, training=True)
dy = y.copy()                      # dL/dy for L = 0.5*sum(y**2)
dx, dgamma, dbeta = bn.backward(dy)

eps_fd = 1e-5
numerical_dx = np.zeros_like(x)
for i in range(x.shape[0]):
    for j in range(x.shape[1]):
        x_plus, x_minus = x.copy(), x.copy()
        x_plus[i, j] += eps_fd; x_minus[i, j] -= eps_fd
        bn_tmp = BatchNorm1D(4); bn_tmp.gamma, bn_tmp.beta = bn.gamma, bn.beta
        Lp = 0.5 * np.sum(bn_tmp.forward(x_plus) ** 2)
        Lm = 0.5 * np.sum(bn_tmp.forward(x_minus) ** 2)
        numerical_dx[i, j] = (Lp - Lm) / (2 * eps_fd)
print("max |analytic - numerical| dx:", np.abs(dx - numerical_dx).max())
```
The finite-difference check should match the analytic `dx` to within floating-point tolerance
(on the order of $10^{-6}$–$10^{-8}$), confirming the Section 2 backward derivation.

## 6. In-Class Exercise
Explain why, with batch size $m=1$, $\sigma_B^2=0$ always (a single point has zero variance from
itself), making BatchNorm's normalization undefined/degenerate — and why LayerNorm (normalizing
across features, not across the batch) has no analogous problem.
