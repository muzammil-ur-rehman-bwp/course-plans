# Week 14 — Lecture Content: Neural Networks I

## 1. The Perceptron
A single artificial neuron: computes a weighted sum of inputs, adds a bias, and applies an
activation function.
```
z = w1*x1 + w2*x2 + ... + wn*xn + b
output = activation(z)
```
A single perceptron can only represent linearly separable functions — famously, it cannot learn
XOR. This motivates stacking neurons into layers (multi-layer networks).

## 2. Activation Functions
| Function | Formula | Typical use |
|---|---|---|
| Sigmoid | 1/(1+e^-z) | Output layer for binary classification (probability) |
| ReLU | max(0, z) | Hidden layers (avoids vanishing gradient better than sigmoid) |
| Softmax | e^zi / sum(e^zj) | Output layer for multi-class classification (probability distribution) |

```python
import numpy as np

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

def relu(z):
    return np.maximum(0, z)

def softmax(z):
    exp_z = np.exp(z - np.max(z))  # subtract max for numerical stability
    return exp_z / exp_z.sum()
```

## 3. Forward Propagation (From Scratch)
```python
# A 2-layer network: input -> hidden (ReLU) -> output (sigmoid)
def forward_pass(x, W1, b1, W2, b2):
    z1 = W1 @ x + b1
    a1 = relu(z1)
    z2 = W2 @ a1 + b2
    a2 = sigmoid(z2)
    return a2
```
This is precisely the `W @ x + b` linear-algebra pattern from Week 3, now chained through
multiple layers with non-linear activations in between — the non-linearity is what lets networks
represent non-linear functions (unlike a single linear/logistic regression model).

## 4. Loss Functions
- **Mean Squared Error**: for regression outputs.
- **Binary Cross-Entropy**: for binary classification outputs (paired with sigmoid).
- **Categorical Cross-Entropy**: for multi-class classification outputs (paired with softmax).

## 5. In-Class Exercise
By hand, compute the forward pass of a tiny 2-input, 2-hidden-unit, 1-output network for a given
input and weight set; then verify the result with the `forward_pass` function above.
