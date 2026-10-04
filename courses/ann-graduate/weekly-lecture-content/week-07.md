# Week 7 — Lecture Content: Optimization Landscape Theory II

## 1. Adam, Re-derived
Adam maintains exponential moving averages of the gradient (first moment) and its elementwise
square (second moment):
$$
m_t = \beta_1 m_{t-1} + (1-\beta_1)g_t, \qquad v_t = \beta_2 v_{t-1} + (1-\beta_2)g_t^2
$$
Because $m_0=v_0=0$, both estimates are biased toward $0$ early in training; Adam corrects this:
$$
\hat m_t = \frac{m_t}{1-\beta_1^t}, \qquad \hat v_t = \frac{v_t}{1-\beta_2^t}, \qquad \theta_t = \theta_{t-1} - \alpha\,\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon}
$$
The per-parameter effective step size is $\alpha/(\sqrt{\hat v_t}+\epsilon)$ — parameters with
historically large gradient magnitude get smaller effective steps, and vice versa, which is why
Adam tends to need less manual learning-rate tuning than plain SGD.

## 2. A Known Convergence Subtlety
Adam's practical success does not mean it is guaranteed to converge even on simple convex
problems. An analysis in the optimization literature (associated with the authors of the AMSGrad
algorithm, who studied Adam's convergence properties in online convex optimization) exhibits a
simple synthetic online convex setting in which Adam's iterates provably fail to converge to the
optimal point. The mechanism: Adam's effective learning rate $\alpha/\sqrt{\hat v_t}$ is *not*
guaranteed to shrink monotonically over time — a large gradient encountered rarely can be
"forgotten" by the exponential moving average $v_t$ within a few steps, letting the effective step
size grow back up right when the optimizer should be settling down. The proposed fix, **AMSGrad**,
replaces $\hat v_t$ with a running **maximum** of all second-moment estimates seen so far,
$\hat v_t^{\max} = \max(\hat v_1,\dots,\hat v_t)$, which guarantees a non-increasing effective
learning rate and restores a convergence guarantee in the convex setting. The lesson for practice
is not "never use Adam" but "adaptive optimizers are not a solved, assumption-free tool" — they
inherit specific, known failure modes from the exact way they adapt their step size.

## 3. Natural Gradient Descent (Conceptual)
Plain gradient descent takes the steepest-descent direction with respect to the **Euclidean**
metric on parameter space: $\Delta\theta = -\alpha\nabla L$. But for a model that defines a
probability distribution $p_\theta$, the "natural" notion of distance between two parameter
settings is not Euclidean distance between $\theta$ and $\theta'$, but how *different* the
distributions $p_\theta$ and $p_{\theta'}$ are (e.g., in KL-divergence). Steepest descent with
respect to this distributional geometry is **natural gradient descent**:
$$
\Delta\theta = -\alpha\, F(\theta)^{-1}\nabla L(\theta), \qquad F(\theta) = \mathbb{E}\big[\nabla\log p_\theta(y\mid x)\,\nabla\log p_\theta(y\mid x)^\top\big]
$$
where $F(\theta)$ is the **Fisher information matrix**. Natural gradient descent is invariant to
reparameterization of $\theta$ in a way plain gradient descent is not, which is conceptually
appealing. It shares exactly the practical problem of Newton's method (Week 6): $F(\theta)$ is a
$p\times p$ matrix, equally infeasible to form or invert at neural-network scale, so it remains
mostly a conceptual benchmark and a motivation for cheaper approximations (e.g., diagonal or
block-diagonal Fisher approximations), not a drop-in training algorithm for large networks.

## 4. Learning-Rate Warmup Theory
**Warmup** linearly (or otherwise smoothly) ramps the learning rate up from near $0$ to its target
value over the first several hundred to several thousand steps, before switching to a normal decay
schedule. Why this helps, especially for adaptive optimizers and normalization-heavy networks:
- Early in training, $v_t$ (Adam's second-moment estimate) is based on very few samples and is a
  high-variance, unreliable estimate of the true gradient-magnitude scale; dividing by
  $\sqrt{\hat v_t}$ when $\hat v_t$ is itself noisy can produce a large, poorly-calibrated
  effective step.
- Normalization layers' running statistics (Week 5) are also poorly estimated in the first few
  steps, so a layer's effective input scale is not yet stable.
- A large step taken on this unstable, early information can push parameters into a region with a
  badly conditioned loss landscape (Week 6) from which recovery is slow or impossible, whereas a
  small initial step lets both the moment estimates and the normalization statistics stabilize
  before the optimizer commits to large moves.

## 5. Code: An Adam Non-Convergence Sketch and a Warmup Comparison
```python
import numpy as np

# --- Part A: a toy "oscillating-gradient" problem in the spirit of the literature's
# non-convergence constructions for adaptive methods. The exact gradients alternate
# between a large, infrequent push in one direction and a small, frequent push back,
# so that a short-memory second-moment estimate can mis-scale the effective step.
def oscillating_grad(t, C=10.0):
    return C if t % 5 == 0 else -1.0

def run_adam(steps=200, beta1=0.9, beta2=0.999, alpha=0.1, eps=1e-8, use_amsgrad=False):
    theta, m, v, v_hat_max = 0.0, 0.0, 0.0, 0.0
    history = []
    for t in range(1, steps + 1):
        g = oscillating_grad(t)
        m = beta1 * m + (1 - beta1) * g
        v = beta2 * v + (1 - beta2) * g * g
        m_hat = m / (1 - beta1 ** t)
        v_hat = v / (1 - beta2 ** t)
        if use_amsgrad:
            v_hat_max = max(v_hat_max, v_hat)
            denom = np.sqrt(v_hat_max) + eps
        else:
            denom = np.sqrt(v_hat) + eps
        theta -= alpha * m_hat / denom
        history.append(theta)
    return history

adam_hist = run_adam(use_amsgrad=False)
amsgrad_hist = run_adam(use_amsgrad=True)
print("Adam      final theta (drifts):", round(adam_hist[-1], 4))
print("AMSGrad   final theta (stabler):", round(amsgrad_hist[-1], 4))

# --- Part B: warmup vs. no warmup on a simple quadratic with a sharp direction
def quadratic_grad(theta):
    return np.array([20.0 * theta[0], 1.0 * theta[1]])   # ill-conditioned: curvature ratio 20:1

def run_with_schedule(warmup_steps, base_lr=0.11, total_steps=60):
    # Stability for coordinate 0 (curvature 20) needs lr < 2/20 = 0.1; base_lr=0.11 exceeds it.
    theta = np.array([1.0, 1.0])
    losses = []
    for t in range(1, total_steps + 1):
        lr = base_lr * min(1.0, t / warmup_steps) if warmup_steps > 0 else base_lr
        theta = theta - lr * quadratic_grad(theta)
        losses.append(10.0 * theta[0] ** 2 + 0.5 * theta[1] ** 2)
    return losses

no_warmup = run_with_schedule(warmup_steps=0)
with_warmup = run_with_schedule(warmup_steps=50)
print("No warmup   final loss (diverges):", no_warmup[-1])
print("With warmup final loss (still small):", round(with_warmup[-1], 6))
```
Part A shows AMSGrad's running maximum preventing the drift that the plain-Adam variant exhibits
under the oscillating-gradient construction; Part B shows how an aggressive fixed learning rate on
an ill-conditioned quadratic can diverge, while the same rate, reached gradually via warmup, does
not.

## 6. In-Class Exercise
Explain, in one or two sentences, why AMSGrad's running-maximum second-moment estimate guarantees
a non-increasing effective learning rate, and why that is exactly the property plain Adam lacks.
