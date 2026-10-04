# Week 1 — Lecture Content: The Deep Learning Landscape and the PyTorch Framework Tour

*Scope note: this week assumes the perceptron, the MLP, the backpropagation derivation, and basic
optimizers (SGD, momentum, RMSProp, Adam) from *Introduction to Artificial Neural Networks* as
known. None of that is re-derived below — it is referenced as prior knowledge.*

## 1. Why Depth Matters

A shallow model must be handed good features; a deep model learns them. Each layer of a deep
network transforms its input into a new representation, and stacking layers lets the network build
increasingly abstract features — edges, then textures, then parts, then objects, in a CNN; local
patterns, then phrases, then meaning, in a sequence model. The prerequisite course already showed
that an MLP with enough hidden units is a universal function approximator; this course is about the
specific architectural choices (convolution, recurrence/gating, attention) that make deep networks
*trainable and effective in practice* on structured data like images, text, and sequences, rather
than treated as a single generic function approximator.

## 2. The Deep Learning Landscape

| Domain | Representative task | Architecture family (covered this semester) |
|---|---|---|
| Vision | Image classification, object detection | CNNs (Weeks 3–5) |
| Language / sequences | Translation, text generation, forecasting | RNN/LSTM/GRU, attention, Transformers (Weeks 6–9) |
| Generative modeling | Generating new images, data | Autoencoders, VAEs, GANs (Weeks 10–12) |
| Practice at scale | Training large models efficiently and reliably | Optimization, transfer learning, debugging (Weeks 5, 13–14) |

## 3. PyTorch Tensors and Autograd

A `torch.Tensor` behaves like a NumPy array but can track gradients:

```python
import torch

x = torch.tensor([2.0, 3.0], requires_grad=True)
w = torch.tensor([1.5, -0.5], requires_grad=True)
b = torch.tensor(0.1, requires_grad=True)

z = (x * w).sum() + b          # z = x0*w0 + x1*w1 + b
y = torch.sigmoid(z)           # same sigmoid activation from the prerequisite course
loss = (y - 1.0) ** 2          # squared error against target 1.0

loss.backward()
print(x.grad, w.grad, b.grad)  # populated automatically via the chain rule
```

`loss.backward()` walks the computational graph PyTorch recorded during the forward pass and
applies the chain rule exactly as the prerequisite course's hand-derivation did — autograd is a
general mechanization of backpropagation, not a different algorithm.

## 4. `nn.Module`: Defining a Model

```python
import torch.nn as nn

class SmallMLP(nn.Module):
    def __init__(self, in_features, hidden, out_features):
        super().__init__()
        self.fc1 = nn.Linear(in_features, hidden)
        self.fc2 = nn.Linear(hidden, out_features)

    def forward(self, x):
        x = torch.relu(self.fc1(x))
        return self.fc2(x)

model = SmallMLP(in_features=4, hidden=8, out_features=2)
```

`nn.Linear(in_features, out_features)` holds a weight matrix and bias vector, initialized
automatically (initialization is studied in depth next week) — this is the same affine
transformation `Wx + b` from the prerequisite course's MLP.

## 5. The Canonical Training Loop

```python
import torch.optim as optim

optimizer = optim.Adam(model.parameters(), lr=1e-3)   # Adam, as covered in the prerequisite course
criterion = nn.CrossEntropyLoss()

for epoch in range(num_epochs):
    for xb, yb in train_loader:
        optimizer.zero_grad()      # clear gradients from the previous step
        logits = model(xb)         # forward pass
        loss = criterion(logits, yb)
        loss.backward()            # backward pass (autograd)
        optimizer.step()           # parameter update
```

Every remaining week of this course reuses this five-line idiom — only the model architecture,
the loss, or the data changes.

## 6. Verifying Autograd Against Prior Knowledge

A useful sanity check when first adopting a framework: reproduce a result already computed by hand
or in NumPy during the prerequisite course, and confirm the numbers match.

```python
# Compare autograd's gradient against a hand-derived gradient for z = x0*w0 + x1*w1 + b, y = sigmoid(z)
# dL/dw0 should equal 2*(y-1)*y*(1-y)*x0 by the chain rule already derived in the prerequisite course.
import math
y_val = y.item()
expected_dw0 = 2 * (y_val - 1) * y_val * (1 - y_val) * x[0].item()
print(expected_dw0, w.grad[0].item())   # should match (up to floating-point precision)
```

## 7. In-Class Exercise

Given the expression `z = x0*w0 + x1*w1 + b` and `y = sigmoid(z)` with provided numeric values,
predict `dL/dw0` and `dL/dw1` by hand using the chain rule, then verify with `.backward()`.
