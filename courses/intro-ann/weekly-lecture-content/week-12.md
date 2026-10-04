# Week 12 — Lecture Content: Introduction to a Deep Learning Framework

## 1. Automatic Differentiation (Autograd)
Every backward-pass formula derived by hand in Week 7 and coded by hand in Week 8
($\delta^{(l)} = (W^{(l+1)\top}\delta^{(l+1)})\odot g'(z^{(l)})$, and the resulting weight/bias
gradients) is a special case of a general technique: **automatic differentiation**. A framework
like PyTorch builds a computational graph of every operation performed on its tensors, then walks
that graph backward — applying the chain rule automatically, operation by operation — when
`.backward()` is called. Framework autograd is not a different algorithm from what was
implemented by hand; it is the *same* chain-rule bookkeeping, generalized to arbitrary
computational graphs instead of one fixed, hand-derived network shape.

```python
import torch

x = torch.tensor([0.5, 0.8], requires_grad=False)
W1 = torch.tensor([[0.1, 0.2], [0.3, 0.4]], requires_grad=True)
b1 = torch.tensor([0.1, 0.1], requires_grad=True)

z1 = W1 @ x + b1
a1 = torch.sigmoid(z1)
loss = a1.sum()
loss.backward()
print(W1.grad)   # dloss/dW1, computed automatically via autograd
```

## 2. Defining a Model
PyTorch models subclass `nn.Module` and define a `forward` method; Keras offers an equivalent
`Sequential` API. Both express the same layer-stacking idea from Week 4's MLP.

```python
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim):
        super().__init__()
        self.fc1 = nn.Linear(in_dim, hidden_dim)
        self.fc2 = nn.Linear(hidden_dim, out_dim)

    def forward(self, x):
        x = torch.relu(self.fc1(x))
        return self.fc2(x)   # raw logits; softmax applied inside the loss function below

model = MLP(in_dim=784, hidden_dim=128, out_dim=10)
```
Note the output layer returns raw logits, not softmax probabilities: PyTorch's
`nn.CrossEntropyLoss` applies `log_softmax` internally for better numerical stability than
applying softmax and then a separate log — the same log-sum-exp stability concern from Week 3/5,
handled once, correctly, inside the framework.

## 3. The Training Loop
```python
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=1e-3)

for epoch in range(num_epochs):
    for X_batch, y_batch in train_loader:
        optimizer.zero_grad()          # clear gradients from the previous step
        logits = model(X_batch)        # forward pass
        loss = criterion(logits, y_batch)
        loss.backward()                # backward pass: autograd computes all gradients
        optimizer.step()               # parameter update, using the chosen optimizer (Week 10)
```
Each line maps directly onto a Week 8/10 concept: `model(X_batch)` is `forward`; `loss.backward()`
is `backward`; `optimizer.step()` is `update`, using whichever optimizer (SGD, momentum, RMSProp,
Adam) was configured — `optim.Adam` implements exactly the bias-corrected update derived in Week
10. `optimizer.zero_grad()` exists because PyTorch *accumulates* gradients into `.grad` by default
across multiple `.backward()` calls — omitting it silently adds each batch's gradient on top of
the previous batch's, corrupting training.

## 4. Training on MNIST
```python
import torch
from torchvision import datasets, transforms
from torch.utils.data import DataLoader

transform = transforms.Compose([transforms.ToTensor(), transforms.Lambda(lambda x: x.view(-1))])
train_data = datasets.MNIST(root="./data", train=True, download=True, transform=transform)
test_data = datasets.MNIST(root="./data", train=False, download=True, transform=transform)
train_loader = DataLoader(train_data, batch_size=64, shuffle=True)
test_loader = DataLoader(test_data, batch_size=256, shuffle=False)

# training loop as above, then evaluate:
model.eval()
correct, total = 0, 0
with torch.no_grad():
    for X_batch, y_batch in test_loader:
        preds = model(X_batch).argmax(dim=1)
        correct += (preds == y_batch).sum().item()
        total += y_batch.size(0)
print(f"Test accuracy: {correct / total:.4f}")
```
`model.eval()` and `torch.no_grad()` disable training-only behavior (such as dropout, if added)
and gradient tracking during evaluation, respectively — directly analogous to Week 11's
`training=False` flag in the from-scratch dropout implementation.

## 5. In-Class Exercise
Starting from the training loop above with `optimizer.zero_grad()` removed, predict what will
happen to the loss over several batches, then run it and confirm. Then restore the line and
confirm normal training resumes.
