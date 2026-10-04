# Week 10 — Lecture Content: Optimizers

## 1. Limitations of Plain SGD
Plain SGD, $\theta \leftarrow \theta - \eta \nabla L(\theta)$, uses only the *current* gradient.
On loss surfaces that are steep in one direction and shallow in another (common in practice), this
forces a learning rate small enough to avoid oscillating in the steep direction, which then makes
progress painfully slow in the shallow direction. Momentum, RMSProp, and Adam all address this by
using *information from past gradients*, not just the current one.

## 2. Momentum
Momentum accumulates a running **velocity** $v$ that combines the current gradient with the
previous velocity, then updates parameters using $v$ instead of the raw gradient directly:

$$
v \leftarrow \alpha v - \eta \nabla L(\theta), \qquad \theta \leftarrow \theta + v
$$

where $\alpha \in [0,1)$ (typically 0.9) controls how much past velocity is retained. Intuitively,
this is like a ball rolling downhill: it builds speed in directions where the gradient
consistently points the same way (shallow, consistent slopes), while oscillating updates in a
steep, flip-flopping direction partially cancel out.

```python
import numpy as np

def momentum_step(theta, grad, v, lr=0.01, alpha=0.9):
    v = alpha * v - lr * grad
    theta = theta + v
    return theta, v
```

## 3. RMSProp
RMSProp keeps a running average $r$ of the **squared** gradient for each parameter, and divides
the learning rate by its square root — giving parameters with historically large gradients a
smaller effective step, and parameters with historically small gradients a larger one:

$$
r \leftarrow \rho r + (1-\rho)\,g \odot g, \qquad \theta \leftarrow \theta - \frac{\eta}{\sqrt{r} + \delta} \odot g
$$

where $g = \nabla L(\theta)$, $\rho$ is typically 0.9, and $\delta$ (e.g. $10^{-8}$) prevents
division by zero.

```python
def rmsprop_step(theta, grad, r, lr=0.001, rho=0.9, delta=1e-8):
    r = rho * r + (1 - rho) * (grad ** 2)
    theta = theta - lr * grad / (np.sqrt(r) + delta)
    return theta, r
```

## 4. Adam
Adam ("Adaptive Moment Estimation," Kingma & Ba, 2014) combines momentum's first-moment (mean)
tracking with RMSProp's second-moment (variance) tracking, and corrects both for their
initialization bias toward zero:

$$
m_t \leftarrow \beta_1 m_{t-1} + (1-\beta_1) g_t, \qquad v_t \leftarrow \beta_2 v_{t-1} + (1-\beta_2)\, g_t \odot g_t
$$
$$
\hat m_t \leftarrow \frac{m_t}{1-\beta_1^t}, \qquad \hat v_t \leftarrow \frac{v_t}{1-\beta_2^t}
$$
$$
\theta_t \leftarrow \theta_{t-1} - \eta\, \frac{\hat m_t}{\sqrt{\hat v_t} + \delta}
$$

Typical defaults: $\beta_1 = 0.9$, $\beta_2 = 0.999$, $\delta = 10^{-8}$, $\eta = 0.001$. The
**bias correction** divisors $1-\beta_1^t$ and $1-\beta_2^t$ matter most early in training:
because $m_0 = v_0 = 0$, the raw $m_t, v_t$ are biased toward zero for small $t$; dividing by
$1-\beta_1^t$ (which is small for small $t$ and approaches 1 as $t$ grows) corrects this bias so
early updates are not artificially shrunk.

```python
def adam_step(theta, grad, m, v, t, lr=0.001, beta1=0.9, beta2=0.999, delta=1e-8):
    m = beta1 * m + (1 - beta1) * grad
    v = beta2 * v + (1 - beta2) * (grad ** 2)
    m_hat = m / (1 - beta1 ** t)
    v_hat = v / (1 - beta2 ** t)
    theta = theta - lr * m_hat / (np.sqrt(v_hat) + delta)
    return theta, m, v
```

Adam is the most widely used default optimizer in practice: it generally converges faster and
more robustly across a wide range of problems than plain SGD or momentum alone, with less
learning-rate tuning required.

## 5. Learning Rate Schedules (Brief)
Even with an adaptive optimizer, it is common to *decrease* $\eta$ over training, since a rate
suited to fast early progress is often too large for fine-grained convergence near a minimum:
- **Step decay:** multiply $\eta$ by a fixed factor (e.g., 0.1) every $N$ epochs.
- **Cosine decay:** smoothly decrease $\eta$ following a cosine curve from an initial value to
  near zero over training.

## 6. In-Class Exercise
For the gradient sequence $g_1=2.0, g_2=1.5, g_3=0.5$ (a single scalar parameter, $\theta_0=0$),
compute Adam's $m_t, v_t, \hat m_t, \hat v_t$, and $\theta_t$ by hand for $t=1,2,3$ using the
default hyperparameters above, and compare the resulting $\theta_3$ to what plain SGD with
$\eta=0.001$ would have produced on the same gradient sequence.
