# Week 3 — Lecture Content: Activation Functions in Depth

## 1. Why Non-Linearity Matters
If every layer of a network applied only a linear transformation, the entire network — no matter
how many layers — would collapse to a single linear function. For two linear layers
$f_1(x) = W_1 x + b_1$ and $f_2(x) = W_2 x + b_2$:

$$
f_2(f_1(x)) = W_2(W_1 x + b_1) + b_2 = (W_2 W_1) x + (W_2 b_1 + b_2)
$$

which is itself of the form $Wx + b$ — a single linear layer could represent the same function.
Depth only buys expressive power once a **non-linear** activation function is inserted between
layers; this is what lets deep networks represent functions a single linear/logistic model
cannot.

## 2. Sigmoid and Tanh
$$
\sigma(z) = \frac{1}{1+e^{-z}}, \qquad \sigma'(z) = \sigma(z)\bigl(1-\sigma(z)\bigr)
$$
$$
\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}, \qquad \tanh'(z) = 1 - \tanh^2(z)
$$

Both squash their input into a bounded range ($(0,1)$ for sigmoid, $(-1,1)$ for tanh) and both
**saturate**: for large $|z|$, the derivative approaches 0. During backpropagation (Week 7), a
near-zero derivative at any layer shrinks the gradient signal passed to earlier layers — the root
cause of the vanishing-gradient problem discussed again in Week 9.

```python
import numpy as np

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

def sigmoid_grad(z):
    s = sigmoid(z)
    return s * (1 - s)

def tanh(z):
    return np.tanh(z)

def tanh_grad(z):
    return 1 - np.tanh(z) ** 2
```

## 3. ReLU and Variants
$$
\text{ReLU}(z) = \max(0, z), \qquad \text{ReLU}'(z) = \begin{cases} 1 & z > 0 \\ 0 & z < 0 \end{cases}
$$

ReLU does not saturate for $z>0$, which greatly reduces vanishing-gradient issues and is why it is
the default choice for hidden layers in most modern networks. Its weakness is the **dying ReLU**
problem: a unit whose weighted input is always negative outputs 0 and has 0 gradient everywhere,
so it stops updating entirely. Two common fixes:

$$
\text{Leaky ReLU}(z) = \begin{cases} z & z>0 \\ \alpha z & z \le 0 \end{cases} \quad (\alpha \approx 0.01)
$$
$$
\text{ELU}(z) = \begin{cases} z & z>0 \\ \alpha(e^z - 1) & z \le 0 \end{cases}
$$

Leaky ReLU and ELU both allow a small, non-zero gradient for negative inputs, keeping units alive.

```python
def relu(z):
    return np.maximum(0, z)

def relu_grad(z):
    return (z > 0).astype(float)

def leaky_relu(z, alpha=0.01):
    return np.where(z > 0, z, alpha * z)

def leaky_relu_grad(z, alpha=0.01):
    return np.where(z > 0, 1.0, alpha)
```

## 4. Softmax
For multi-class classification, the output layer uses softmax to convert raw scores ("logits")
$z_1,\dots,z_K$ into a probability distribution over $K$ classes:

$$
\text{softmax}(z)_i = \frac{e^{z_i}}{\sum_{j=1}^K e^{z_j}}
$$

Subtracting $\max(z)$ before exponentiating avoids numerical overflow without changing the result
(since it cancels in the ratio):

```python
def softmax(z):
    shifted = z - np.max(z)
    exp_z = np.exp(shifted)
    return exp_z / np.sum(exp_z)
```

Sigmoid is the special case of softmax for $K=2$ classes, parameterized by a single logit instead
of two.

## 5. Choosing an Activation
| Layer | Typical choice | Why |
|---|---|---|
| Hidden layers | ReLU (default), Leaky ReLU/ELU if dying units are observed | Avoids saturation; cheap to compute |
| Output — binary classification | Sigmoid | Outputs a single probability in $(0,1)$ |
| Output — multi-class classification | Softmax | Outputs a probability distribution over $K$ classes |
| Output — regression | None (identity/linear) | Unbounded real-valued targets |

## 6. In-Class Exercise
Plot sigmoid, tanh, and ReLU together with their derivatives on the range $z \in [-6, 6]$. Identify,
from the derivative plots alone, the input range over which each function saturates, and discuss
which activation would suffer most from vanishing gradients in a very deep network.
