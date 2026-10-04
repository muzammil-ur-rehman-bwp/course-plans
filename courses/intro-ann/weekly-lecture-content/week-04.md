# Week 4 — Lecture Content: The Multi-Layer Perceptron (MLP)

## 1. Architecture
An MLP arranges units into an **input layer**, one or more **hidden layers**, and an **output
layer**, with every unit in one layer connected to every unit in the next ("fully connected").
Layer $l$ has weight matrix $W^{(l)}$ of shape $(n_l, n_{l-1})$ and bias vector $b^{(l)}$ of shape
$(n_l,)$, where $n_l$ is the number of units in layer $l$.

## 2. Forward Propagation in Matrix Form
Let $a^{(0)} = x$ (the input). For each layer $l = 1, \dots, L$:

$$
z^{(l)} = W^{(l)} a^{(l-1)} + b^{(l)}, \qquad a^{(l)} = g^{(l)}\bigl(z^{(l)}\bigr)
$$

where $g^{(l)}$ is that layer's activation function (ReLU for hidden layers; sigmoid/softmax/
identity for the output layer, per Week 3). The final $a^{(L)}$ is the network's output,
$\hat y$. This is exactly the two-layer `forward_pass` pattern from the prior course's Week 14,
generalized to an arbitrary number of layers $L$.

```python
import numpy as np

def relu(z):
    return np.maximum(0, z)

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

def forward_pass(x, weights, biases, activations):
    """
    weights:    list of W^(l), activations: list of per-layer activation functions
    biases:     list of b^(l)
    """
    a = x
    for W, b, g in zip(weights, biases, activations):
        z = W @ a + b
        a = g(z)
    return a
```

## 3. Solving XOR With an MLP
A 2-input, 2-hidden-unit, 1-output network *can* solve XOR exactly, with hand-chosen weights (step
activation on the hidden layer for clarity):

$$
h_1 = \text{step}(x_1 + x_2 - 0.5), \qquad h_2 = \text{step}(x_1 + x_2 - 1.5)
$$
$$
\hat y = \text{step}(h_1 - h_2 - 0.5)
$$

Here $h_1$ fires like OR (fires if at least one input is 1) and $h_2$ fires like AND (fires only
if both are 1); the output layer computes $h_1 \text{ AND NOT } h_2$, which is exactly XOR:

```python
def step(z):
    return (z >= 0).astype(float)

W1 = np.array([[1, 1], [1, 1]])      # shape (2 hidden units, 2 inputs)
b1 = np.array([-0.5, -1.5])
W2 = np.array([[1, -1]])             # shape (1 output, 2 hidden units)
b2 = np.array([-0.5])

for x1 in (0, 1):
    for x2 in (0, 1):
        x = np.array([x1, x2], dtype=float)
        out = forward_pass(x, [W1, W2], [b1, b2], [step, step])
        print(x1, x2, "->", out)   # matches XOR exactly
```

This is the concrete resolution of Week 2's XOR problem: no *single* perceptron can learn XOR, but
a network of two perceptron-like units feeding a third one computes it exactly. The weights above
were hand-designed for illustration; Weeks 6–8 show how a network *learns* such weights from data
instead of having them hand-derived.

## 4. The Universal Approximation Theorem (Conceptual)
The Universal Approximation Theorem states that a feedforward network with a single hidden layer
containing enough units, using a suitable non-linear activation, can approximate any continuous
function on a bounded input domain to arbitrary accuracy. Two caveats matter as much as the
headline claim:
1. It is an **existence** result — it says such weights exist, not that gradient descent will
   find them, nor how many hidden units "enough" requires in practice.
2. In practice, networks that are **deeper** (more layers, each modestly sized) are often far more
   efficient at representing complex functions than a single very wide hidden layer, which is why
   modern networks favor depth over extreme width.

## 5. In-Class Exercise
Using the `forward_pass` function, verify by hand (writing out each layer's $z$ and $a$) and then
in code that the XOR network above produces the correct output for all four input combinations.
Then modify the network to add a bias term that would break it for exactly one input, and predict
which input fails before running the code.
