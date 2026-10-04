# Week 8 — Lecture Content: Backpropagation From Scratch in NumPy

## 1. From Derivation to Code
Week 7 derived, for a 2-layer network with sigmoid activations and binary cross-entropy loss:
$\delta^{(2)} = \hat y - y$, $\delta^{(1)} = (W^{(2)\top}\delta^{(2)}) \odot a^{(1)}(1-a^{(1)})$,
and $\partial L/\partial W^{(l)} = \delta^{(l)}(a^{(l-1)})^\top$, $\partial L/\partial b^{(l)} =
\delta^{(l)}$. This section implements exactly those formulas, generalized to a batch of
examples, as a reusable class.

## 2. A Full `NeuralNetwork` Class
```python
import numpy as np

def sigmoid(z):
    return 1 / (1 + np.exp(-z))

class NeuralNetwork:
    def __init__(self, n_in, n_hidden, n_out, seed=0):
        rng = np.random.default_rng(seed)
        self.W1 = rng.normal(0, 0.5, size=(n_hidden, n_in))
        self.b1 = np.zeros((n_hidden, 1))
        self.W2 = rng.normal(0, 0.5, size=(n_out, n_hidden))
        self.b2 = np.zeros((n_out, 1))

    def forward(self, X):
        # X has shape (n_in, n_examples); each column is one example
        self.X = X
        self.Z1 = self.W1 @ X + self.b1
        self.A1 = sigmoid(self.Z1)
        self.Z2 = self.W2 @ self.A1 + self.b2
        self.A2 = sigmoid(self.Z2)
        return self.A2

    def backward(self, Y):
        m = Y.shape[1]                                  # number of examples in the batch
        dZ2 = self.A2 - Y                                # delta^(2), averaged over the batch below
        dW2 = (dZ2 @ self.A1.T) / m
        db2 = np.sum(dZ2, axis=1, keepdims=True) / m
        dA1 = self.W2.T @ dZ2
        dZ1 = dA1 * self.A1 * (1 - self.A1)              # delta^(1)
        dW1 = (dZ1 @ self.X.T) / m
        db1 = np.sum(dZ1, axis=1, keepdims=True) / m
        return dW1, db1, dW2, db2

    def update(self, grads, lr):
        dW1, db1, dW2, db2 = grads
        self.W1 -= lr * dW1
        self.b1 -= lr * db1
        self.W2 -= lr * dW2
        self.b2 -= lr * db2

def bce_loss(Y_hat, Y, eps=1e-12):
    Y_hat = np.clip(Y_hat, eps, 1 - eps)
    return -np.mean(Y * np.log(Y_hat) + (1 - Y) * np.log(1 - Y_hat))
```
The batch averaging (`/ m`) is the only addition beyond Week 7's single-example derivation: each
gradient is averaged across the mini-batch, consistent with Week 6's mini-batch gradient descent
and Week 5's loss already being a per-batch mean.

## 3. Training Loop on XOR
```python
X = np.array([[0, 0, 1, 1],
              [0, 1, 0, 1]])              # shape (2, 4): each column one example
Y = np.array([[0, 1, 1, 0]])              # shape (1, 4)

net = NeuralNetwork(n_in=2, n_hidden=4, n_out=1, seed=0)
losses = []
for epoch in range(5000):
    Y_hat = net.forward(X)
    loss = bce_loss(Y_hat, Y)
    losses.append(loss)
    grads = net.backward(Y)
    net.update(grads, lr=0.5)

print("Final predictions:", net.forward(X))
print("Final loss:", losses[-1])
```
With enough hidden units (here, 4) and enough epochs, this from-scratch network — unlike the
single perceptron in Week 2 — reliably drives the loss toward zero and classifies all four XOR
points correctly, concretely demonstrating that depth plus a working training algorithm resolves
the limitation Week 2 identified and Week 4 only hand-engineered a fix for.

## 4. Reading the Loss Curve
Plotting `losses` vs. epoch should show a curve that decreases, generally monotonically (small
oscillations are normal with a fixed learning rate), flattening as the network approaches a good
solution. A curve that does not decrease at all signals a bug — most commonly a transpose error in
the gradient formulas, a learning rate far too high or low, or a label/shape mismatch — exactly
the gradient-checking skill from Week 7's lab is the right tool to isolate which.

## 5. Midterm Review (Weeks 1–8)
Review spans: the McCulloch-Pitts neuron and history (Wk 1); the perceptron and linear
separability (Wk 2); activation functions and their derivatives (Wk 3); the MLP and matrix-form
forward propagation (Wk 4); loss functions and the loss/activation pairing (Wk 5); gradient
descent variants (Wk 6); the backpropagation derivation (Wk 7). Students should be able to
derive, not just recite, every formula used in this week's `NeuralNetwork` class.

## 6. In-Class Exercise
Modify the training loop to print the loss every 500 epochs; observe and explain the shape of the
decreasing curve. Then intentionally introduce a transpose bug into `backward` (swap `dZ2 @
self.A1.T` for `self.A1 @ dZ2.T` where shapes allow) and observe how the loss curve's behavior
changes — connecting directly back to Week 7's lab notes on this exact bug class.
