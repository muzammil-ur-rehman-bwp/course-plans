# Week 5 — Lecture Content: Loss Functions

## 1. Why a Loss Function
A loss function $L(\hat y, y)$ turns a network's prediction $\hat y$ and the true label $y$ into
a single scalar that is small when the prediction is good and large when it is bad. Training
(Weeks 6–8) is the process of adjusting weights to make this scalar small, on average, across the
training set.

## 2. Mean Squared Error (Regression)
For a scalar or vector target:

$$
L_{\text{MSE}}(\hat y, y) = \frac{1}{n}\sum_{i=1}^n (\hat y_i - y_i)^2
$$

Used when the output is a real-valued, unbounded quantity (e.g., predicting a price), paired with
an identity (linear) output activation.

```python
import numpy as np

def mse(y_hat, y):
    return np.mean((y_hat - y) ** 2)
```

## 3. Binary Cross-Entropy (Binary Classification)
For a single probability output $\hat y \in (0,1)$ (from a sigmoid) and true label $y \in \{0,1\}$:

$$
L_{\text{BCE}}(\hat y, y) = -\bigl[y\log(\hat y) + (1-y)\log(1-\hat y)\bigr]
$$

This is the negative log-likelihood of the correct label under a Bernoulli model with parameter
$\hat y$ — it penalizes a confident, wrong prediction ($\hat y \to 0$ when $y=1$) far more
severely than MSE does, because $-\log(\hat y) \to \infty$ as $\hat y \to 0$.

```python
def binary_cross_entropy(y_hat, y, eps=1e-12):
    y_hat = np.clip(y_hat, eps, 1 - eps)  # avoid log(0)
    return -np.mean(y * np.log(y_hat) + (1 - y) * np.log(1 - y_hat))
```

## 4. Categorical Cross-Entropy (Multi-Class Classification)
For a $K$-class probability vector $\hat y$ (from softmax) and a one-hot true label vector $y$:

$$
L_{\text{CCE}}(\hat y, y) = -\sum_{k=1}^K y_k \log(\hat y_k) = -\log(\hat y_{k^*})
$$

where $k^*$ is the true class index — because $y$ is one-hot, every term except the true class's
vanishes, leaving just the negative log-probability the model assigned to the correct class.

```python
def categorical_cross_entropy(y_hat, y_onehot, eps=1e-12):
    y_hat = np.clip(y_hat, eps, 1 - eps)
    return -np.mean(np.sum(y_onehot * np.log(y_hat), axis=-1))
```

## 5. Why the Loss Must Match the Output Activation: MSE vs. Cross-Entropy at Saturation
Consider a sigmoid output $\hat y = \sigma(z)$ with true label $y=1$, and the network confidently
(and wrongly) predicts $\hat y \approx 0$ (i.e., $z \ll 0$, deep in sigmoid's saturated region).
The gradient of the loss with respect to $z$ is, by the chain rule:

$$
\frac{\partial L}{\partial z} = \frac{\partial L}{\partial \hat y}\cdot\frac{\partial \hat y}{\partial z} = \frac{\partial L}{\partial \hat y}\cdot \sigma(z)\bigl(1-\sigma(z)\bigr)
$$

For **MSE**, $\partial L/\partial \hat y = 2(\hat y - y)$, so:
$$
\frac{\partial L_{\text{MSE}}}{\partial z} = 2(\hat y - y)\,\sigma(z)(1-\sigma(z))
$$
Near saturation, $\sigma(z)(1-\sigma(z)) \to 0$, so **this gradient vanishes** even though the
prediction is badly wrong — the network barely updates, learning very slowly from its worst
mistakes.

For **binary cross-entropy**, $\partial L_{\text{BCE}}/\partial \hat y = \frac{\hat y - y}{\hat
y(1-\hat y)}$, so the $\sigma(z)(1-\sigma(z))$ terms **cancel exactly**:
$$
\frac{\partial L_{\text{BCE}}}{\partial z} = \frac{\hat y - y}{\hat y (1-\hat y)}\cdot \hat y(1-\hat y) = \hat y - y
$$
This gradient is simply the prediction error, with no vanishing term — a confidently wrong
prediction produces a large, correcting gradient. This cancellation is precisely why
cross-entropy, not MSE, is the standard loss for classification outputs, and it is a concrete,
derivable reason — not just a convention — that will reappear in Week 7's backpropagation
derivation as a strikingly simple $\partial L/\partial z$ term at the output layer.

## 6. In-Class Exercise
For $y=1$ and $\hat y \in \{0.01, 0.5, 0.99\}$, compute both $L_{\text{MSE}}$ and $L_{\text{BCE}}$
by hand, and compute $\partial L/\partial z$ for both losses at $\hat y = 0.01$ (using $z$ such
that $\sigma(z)=0.01$). Confirm numerically that the cross-entropy gradient is far larger than the
MSE gradient at this confidently-wrong point.
