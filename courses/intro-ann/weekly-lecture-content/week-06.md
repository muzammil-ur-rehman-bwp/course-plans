# Week 6 — Lecture Content: Gradient Descent

## 1. The Gradient
For a scalar loss $L(\theta)$ depending on parameters $\theta = (\theta_1,\dots,\theta_p)$, the
gradient $\nabla L(\theta) = \left(\frac{\partial L}{\partial \theta_1}, \dots,
\frac{\partial L}{\partial \theta_p}\right)$ points in the direction of **steepest increase** of
$L$ at $\theta$. Moving in the *opposite* direction, $-\nabla L(\theta)$, is therefore the
direction of steepest *decrease* — the basis of gradient descent.

## 2. The Gradient Descent Update Rule
$$
\theta \leftarrow \theta - \eta \nabla L(\theta)
$$
where $\eta > 0$ is the **learning rate**, controlling the step size. Applied repeatedly, this
iteratively moves $\theta$ toward a point where $\nabla L(\theta) \approx 0$ (a local minimum, or
for a convex loss, the global minimum).

```python
import numpy as np

def gradient_descent(grad_fn, theta0, lr=0.1, steps=50):
    theta = theta0.copy()
    history = [theta.copy()]
    for _ in range(steps):
        theta = theta - lr * grad_fn(theta)
        history.append(theta.copy())
    return theta, np.array(history)

# Example: L(theta) = (theta - 3)^2, so dL/dtheta = 2*(theta - 3)
grad_fn = lambda theta: 2 * (theta - 3)
theta_final, path = gradient_descent(grad_fn, np.array([0.0]), lr=0.1, steps=50)
print(theta_final)  # converges toward 3.0
```

## 3. Effect of the Learning Rate
- **Too small**: convergence is correct but very slow — many steps needed.
- **Well-chosen**: smooth, efficient convergence to the minimum.
- **Too large**: steps overshoot the minimum; the loss can oscillate or diverge entirely. For the
  1D quadratic $L(\theta)=(\theta-3)^2$, the update becomes
  $\theta_{t+1} - 3 = (1-2\eta)(\theta_t - 3)$ — divergence occurs once $|1-2\eta| > 1$, i.e.
  $\eta > 1$ for this particular loss's curvature.

```python
for lr in (0.05, 0.5, 1.1):
    theta_final, path = gradient_descent(grad_fn, np.array([0.0]), lr=lr, steps=20)
    print(f"lr={lr}: final theta = {theta_final}, |error| = {abs(theta_final[0] - 3):.4f}")
```

## 4. Batch, Stochastic, and Mini-Batch Gradient Descent
For a training set of $N$ examples, the true loss is an average over all examples:
$L(\theta) = \frac{1}{N}\sum_{i=1}^N L_i(\theta)$, so $\nabla L(\theta) = \frac{1}{N}\sum_i
\nabla L_i(\theta)$.

| Variant | Gradient computed from | Trade-off |
|---|---|---|
| **Batch gradient descent** | All $N$ examples | Exact gradient, but one update per full pass over data — slow for large $N$ |
| **Stochastic gradient descent (SGD)** | 1 randomly chosen example | Very fast updates, but each is a noisy estimate of the true gradient |
| **Mini-batch gradient descent** | A small batch (e.g., 32–256 examples) | Balances update speed and gradient-estimate quality; the standard choice in practice |

```python
def mini_batch_gradient_descent(X, y, grad_fn, theta0, lr=0.1, batch_size=32, epochs=10):
    theta = theta0.copy()
    n = X.shape[0]
    for epoch in range(epochs):
        perm = np.random.permutation(n)          # shuffle every epoch
        X_shuffled, y_shuffled = X[perm], y[perm]
        for start in range(0, n, batch_size):
            X_batch = X_shuffled[start:start + batch_size]
            y_batch = y_shuffled[start:start + batch_size]
            theta = theta - lr * grad_fn(theta, X_batch, y_batch)
    return theta
```

## 5. Why Shuffling Matters
If data is not shuffled and happens to be sorted or grouped by label or by some other pattern,
each mini-batch becomes a biased, non-representative sample of the full dataset — gradients
computed from it systematically point in the "wrong" direction relative to the true gradient,
slowing or distorting convergence. Re-shuffling every epoch (as in the loop above) ensures each
epoch's batches are an unbiased sample of the full training set.

## 6. In-Class Exercise
By hand, for $L(\theta) = (\theta - 3)^2$, $\theta_0 = 0$, compute $\theta_1$ and $\theta_2$ for
$\eta = 0.1$. Then, without computing further, predict (using the divergence condition above)
whether $\eta = 1.2$ will converge or diverge, and verify with the code.
