# Week 15 — Lecture Content: Neural Networks II — Backpropagation & Training

## 1. Backpropagation Intuition
Backpropagation computes the gradient of the loss with respect to every weight in the network by
applying the **chain rule** backward from the output layer to the input layer. Conceptually:
1. Forward pass: compute predictions and the loss.
2. Backward pass: propagate the error gradient backward, layer by layer, computing how much each
   weight contributed to the error.
3. Update each weight in the direction that reduces the loss (gradient descent step).

We walk through the chain rule by hand on the tiny network from Week 14 to make the backward
pass concrete before relying on a framework's automatic differentiation.

## 2. Optimizers
| Optimizer | Idea |
|---|---|
| SGD | Update weights using the gradient from a single batch, scaled by a learning rate |
| SGD + Momentum | Adds a "velocity" term so updates build up in a consistent direction |
| Adam | Adapts the learning rate per-parameter using running estimates of gradient mean/variance |

Adam is the most commonly used default in practice due to generally faster, more stable
convergence across a wide range of problems.

## 3. Building a Model in PyTorch (illustrative)
```python
import torch
import torch.nn as nn

class SimpleNet(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim):
        super().__init__()
        self.fc1 = nn.Linear(in_dim, hidden_dim)
        self.fc2 = nn.Linear(hidden_dim, out_dim)

    def forward(self, x):
        x = torch.relu(self.fc1(x))
        return self.fc2(x)

model = SimpleNet(784, 128, 10)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
```

## 4. Training Loop
```python
for epoch in range(num_epochs):
    optimizer.zero_grad()
    outputs = model(X_batch)
    loss = criterion(outputs, y_batch)
    loss.backward()   # backpropagation (autodiff)
    optimizer.step()  # weight update
```
`loss.backward()` is where the framework performs backpropagation automatically — students have
already done this by hand in Week 14/15's lecture to understand what it is computing.

## 5. Reading Loss Curves
- Both training and validation loss decreasing and converging → healthy training.
- Training loss keeps decreasing but validation loss rises → overfitting (Week 13 concepts
  apply directly to neural networks too).
- Loss not decreasing at all → learning rate likely too high/low, or a bug in data/labels.

## 6. In-Class Exercise
Train the `SimpleNet` model on a small image dataset (e.g., MNIST subset) for a few epochs, plot
training/validation loss curves, and report test accuracy.
