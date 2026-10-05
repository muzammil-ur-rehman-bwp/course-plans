# Week 15: Neural Networks II, Backpropagation and Training

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain backpropagation as the chain rule applied from the output back to the input.
2. Compute the gradients of a small network by hand, and check them numerically.
3. Compare SGD, SGD with momentum and Adam.
4. Define a network in PyTorch, and write a correct training loop with mini-batches.
5. Read training and validation loss curves, and diagnose healthy training, overfitting and a stalled network.

## 1. Backpropagation Intuition

Last week we computed the output of a network. To train it, we need to know how to change every weight so that the loss falls. The loss depends on the output, which depends on the hidden layer, which depends on the first layer of weights, so a change in an early weight travels through many steps before reaching the loss. The chain rule of calculus tells us how to combine these effects: the derivative of a composition is the product of the derivatives of its parts.

Backpropagation is simply an efficient way to apply the chain rule through the whole network. It has three stages.

1. Forward pass. Compute all the intermediate values and the loss, and remember them.
2. Backward pass. Starting from the loss, move backwards layer by layer. At each layer compute how much the loss changes with respect to that layer's inputs and its weights, reusing the result from the layer after it.
3. Update. Move each weight a small step in the direction that reduces the loss.

The efficiency comes from reuse. Each layer needs the gradient arriving from the layer above, and multiplies it by its own local derivative. The cost of the backward pass is of the same order as the forward pass.

### 1.1 The chain rule by hand

We use the network from Week 14, with its input `x = [1, 2]`, hidden ReLU layer and sigmoid output, and a true label of `y = 1`. The weights are the same as before.

```
z1 = W1 x + b1        a1 = relu(z1)
z2 = W2 a1 + b2       p  = sigmoid(z2)
L  = -[y log p + (1 - y) log(1 - p)]
```

Work backwards.

1. For a sigmoid output with binary cross-entropy loss, the derivative of the loss with respect to `z2` simplifies neatly to `dz2 = p - y`. With `p = 0.0911` and `y = 1` this is `-0.9089`.
2. The output weights: `dW2 = dz2 * a1 = -0.9089 * [0.2, 1.8] = [-0.1818, -1.6360]`, and `db2 = dz2 = -0.9089`.
3. Pass the error back to the hidden layer: `da1 = W2^T dz2 = [1.0, -1.5] * (-0.9089) = [-0.9089, 1.3634]`.
4. Through the ReLU: the derivative is 1 where `z1 > 0` and 0 elsewhere. Both hidden values are positive, so `dz1 = da1 = [-0.9089, 1.3634]`.
5. First layer weights: `dW1 = dz1 x^T`, which is the outer product, approximately `[[-0.9089, -1.8178], [1.3634, 2.7267]]`, and `db1 = dz1`.

Now the same calculation in code, and a check against a numerical derivative.

```python
import numpy as np

def relu(z): return np.maximum(0, z)
def sigmoid(z): return 1 / (1 + np.exp(-z))

W1 = np.array([[0.5, -0.2], [0.3, 0.8]])
b1 = np.array([0.1, -0.1])
W2 = np.array([[1.0, -1.5]])
b2 = np.array([0.2])
x = np.array([1.0, 2.0])
y = 1.0

def loss_fn(W1, b1, W2, b2):
    a1 = relu(W1 @ x + b1)
    p = sigmoid(W2 @ a1 + b2)[0]
    return -(y * np.log(p) + (1 - y) * np.log(1 - p))

# forward pass, keeping intermediate values
z1 = W1 @ x + b1
a1 = relu(z1)
z2 = W2 @ a1 + b2
p = sigmoid(z2)[0]

# backward pass
dz2 = p - y
dW2 = dz2 * a1.reshape(1, -1)
db2 = np.array([dz2])
da1 = W2.T[:, 0] * dz2
dz1 = da1 * (z1 > 0)
dW1 = np.outer(dz1, x)
db1 = dz1

print("dW2:", dW2.round(4))
print("dW1:\n", dW1.round(4))
```

The numbers should agree with the hand calculation. A numerical gradient check is the standard way to catch mistakes in backpropagation code. We nudge one weight by a tiny amount in both directions and measure the change in the loss.

```python
def numeric_grad(param_name, eps=1e-6):
    params = {"W1": W1.copy(), "b1": b1.copy(), "W2": W2.copy(), "b2": b2.copy()}
    grad = np.zeros_like(params[param_name])
    it = np.nditer(params[param_name], flags=["multi_index"])
    for _ in it:
        idx = it.multi_index
        original = params[param_name][idx]
        params[param_name][idx] = original + eps
        up = loss_fn(**params)
        params[param_name][idx] = original - eps
        down = loss_fn(**params)
        params[param_name][idx] = original
        grad[idx] = (up - down) / (2 * eps)
    return grad

for name, analytic in [("W1", dW1), ("b1", db1), ("W2", dW2), ("b2", db2)]:
    num = numeric_grad(name)
    print(f"{name}: max difference {np.abs(num - analytic).max():.2e}")
```

All the differences should be tiny, around `1e-10`. If a difference were large, our hand-derived formula would be wrong. Real frameworks have automatic differentiation, so we normally never write these formulas, but the check is still worth doing when you implement a custom layer.

### 1.2 A full training loop from scratch

Using these gradients, we can train the network by gradient descent. Here it learns XOR, which a single neuron could not.

```python
rng = np.random.default_rng(1)
X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]], dtype=float)
Y = np.array([0, 1, 1, 0], dtype=float)

H = 4
W1 = rng.normal(0, 1, size=(H, 2)); b1 = np.zeros(H)
W2 = rng.normal(0, 1, size=(1, H)); b2 = np.zeros(1)
lr = 0.5

for epoch in range(3001):
    Z1 = X @ W1.T + b1               # (4, H)
    A1 = relu(Z1)
    Z2 = A1 @ W2.T + b2              # (4, 1)
    P = sigmoid(Z2)[:, 0]
    loss = -np.mean(Y * np.log(P + 1e-12) + (1 - Y) * np.log(1 - P + 1e-12))

    dZ2 = (P - Y).reshape(-1, 1) / len(Y)
    dW2 = dZ2.T @ A1
    db2 = dZ2.sum(axis=0)
    dA1 = dZ2 @ W2
    dZ1 = dA1 * (Z1 > 0)
    dW1 = dZ1.T @ X
    db1 = dZ1.sum(axis=0)

    W1 -= lr * dW1; b1 -= lr * db1
    W2 -= lr * dW2; b2 -= lr * db2
    if epoch % 1000 == 0:
        print(f"epoch {epoch:4d}  loss {loss:.4f}")

print("predictions:", P.round(3))
```

The loss falls and the predictions approach 0, 1, 1, 0. With some random initializations a ReLU network of this size can get stuck, so if your run does not converge, change the seed. That sensitivity to initialization is a real feature of neural network training, and not a bug in your code.

## 2. Optimizers

Plain gradient descent uses the whole dataset for every step. For large datasets this is slow, so we use mini-batches: small random subsets, typically 32 to 256 examples. The gradient from a batch is a noisy estimate of the full gradient, but it is much cheaper, and the noise often helps. A full pass through the data is called an epoch.

1. SGD (stochastic gradient descent): `w := w - lr * gradient`, using the gradient from a mini-batch. Simple, but sensitive to the learning rate.
2. SGD with momentum: keep a running velocity, `v := beta * v + gradient`, and move by `w := w - lr * v`. Updates build up in directions that are consistent and cancel in directions that oscillate. Think of a heavy ball rolling downhill.
3. Adam: keeps running estimates of both the mean and the variance of each parameter's gradient, and scales the step for each parameter individually. It is robust to the choice of learning rate, and a good default is 1e-3.

The differences are easiest to see on a toy problem with a narrow valley, where plain SGD zig-zags.

```python
def grad(w):
    # f(w) = 0.5 * (w0^2 + 25 * w1^2): a narrow valley
    return np.array([w[0], 25 * w[1]])

def f(w):
    return 0.5 * (w[0] ** 2 + 25 * w[1] ** 2)

def sgd(steps, lr):
    w = np.array([5.0, 1.0])
    for _ in range(steps):
        w = w - lr * grad(w)
    return f(w)

def momentum(steps, lr, beta=0.9):
    w, v = np.array([5.0, 1.0]), np.zeros(2)
    for _ in range(steps):
        v = beta * v + grad(w)
        w = w - lr * v
    return f(w)

def adam(steps, lr, b1=0.9, b2=0.999, eps=1e-8):
    w = np.array([5.0, 1.0])
    m, v = np.zeros(2), np.zeros(2)
    for t in range(1, steps + 1):
        g = grad(w)
        m = b1 * m + (1 - b1) * g
        v = b2 * v + (1 - b2) * g ** 2
        m_hat, v_hat = m / (1 - b1 ** t), v / (1 - b2 ** t)
        w = w - lr * m_hat / (np.sqrt(v_hat) + eps)
    return f(w)

print("final loss after 100 steps")
print("  SGD      lr=0.03 :", f"{sgd(100, 0.03):.2e}")
print("  Momentum lr=0.01 :", f"{momentum(100, 0.01):.2e}")
print("  Adam     lr=0.3  :", f"{adam(100, 0.3):.2e}")
```

Do not read too much into one toy run. The point is that each method has its own sensible learning rate, and it is worth tuning it. In practice, Adam is the most common default thanks to its fast and stable behaviour across many problems, and SGD with momentum is still popular for large image models when carefully tuned.

## 3. Building a Model in PyTorch

PyTorch implements arrays on the GPU or CPU (tensors), automatic differentiation, and a library of layers and optimizers. The code in this section needs `pip install torch`. The workflow follows exactly what we did by hand.

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

model = SimpleNet(64, 32, 10)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

print(model)
print("parameters:", sum(p.numel() for p in model.parameters()))
```

This is the inheritance idea from Week 2 in action. We subclass `nn.Module`, create the layers in `__init__`, and describe the forward computation in `forward`. PyTorch works out the backward pass by itself.

Notice that the network returns raw scores, called logits, without a softmax at the end. `nn.CrossEntropyLoss` applies the softmax internally in a numerically stable way, and it expects integer class labels, not one-hot vectors. Adding a softmax layer yourself before this loss is a common error.

### 3.1 Autograd checks our hand calculation

PyTorch can reproduce the gradients we derived by hand.

```python
W1t = torch.tensor([[0.5, -0.2], [0.3, 0.8]], requires_grad=True)
b1t = torch.tensor([0.1, -0.1], requires_grad=True)
W2t = torch.tensor([[1.0, -1.5]], requires_grad=True)
b2t = torch.tensor([0.2], requires_grad=True)
xt = torch.tensor([1.0, 2.0])

a1 = torch.relu(W1t @ xt + b1t)
p = torch.sigmoid(W2t @ a1 + b2t)
loss = -torch.log(p).sum()          # the label is 1
loss.backward()

print("autograd dW2:", W2t.grad.numpy().round(4))
print("autograd dW1:\n", W1t.grad.numpy().round(4))
```

The numbers match the hand derivation, which is the whole point. `loss.backward()` is performing the backpropagation we did step by step.

## 4. The Training Loop

The structure is the same for almost every PyTorch model.

```
for epoch in range(num_epochs):
    for X_batch, y_batch in loader:
        optimizer.zero_grad()
        outputs = model(X_batch)
        loss = criterion(outputs, y_batch)
        loss.backward()    # backpropagation (autodiff)
        optimizer.step()   # weight update
```

1. `zero_grad()` clears the gradients from the previous step. PyTorch accumulates gradients by default, so forgetting this line is a classic bug that makes training behave strangely.
2. The forward pass computes outputs and the loss.
3. `backward()` computes the gradients of the loss with respect to every parameter.
4. `step()` updates the parameters using those gradients.

Here is a complete runnable example. We use the handwritten digits dataset that comes with scikit-learn, with 1,797 images of 8 by 8 pixels. It is a small relative of MNIST and needs no download. If you have torchvision installed and an internet connection, the same code works for MNIST after changing the input size to 784.

```python
import numpy as np
import torch
import torch.nn as nn
from sklearn.datasets import load_digits
from sklearn.model_selection import train_test_split

torch.manual_seed(0)
np.random.seed(0)

digits = load_digits()
X = (digits.data / 16.0).astype(np.float32)      # pixel values are 0..16
y = digits.target.astype(np.int64)

X_trainval, X_test, y_trainval, y_test = train_test_split(
    X, y, test_size=0.2, random_state=0, stratify=y)
X_train, X_val, y_train, y_val = train_test_split(
    X_trainval, y_trainval, test_size=0.25, random_state=0, stratify=y_trainval)

Xtr, ytr = torch.from_numpy(X_train), torch.from_numpy(y_train)
Xva, yva = torch.from_numpy(X_val), torch.from_numpy(y_val)
Xte, yte = torch.from_numpy(X_test), torch.from_numpy(y_test)
print("train / val / test:", len(Xtr), len(Xva), len(Xte))
```

We split into training, validation and test sets, as the previous weeks taught us. The validation set is for watching the training and choosing when to stop, and the test set is for one final number.

```python
from torch.utils.data import TensorDataset, DataLoader

loader = DataLoader(TensorDataset(Xtr, ytr), batch_size=32, shuffle=True)

model = SimpleNet(64, 32, 10)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

train_losses, val_losses, val_accs = [], [], []
for epoch in range(40):
    model.train()
    running = 0.0
    for xb, yb in loader:
        optimizer.zero_grad()
        loss = criterion(model(xb), yb)
        loss.backward()
        optimizer.step()
        running += loss.item() * len(xb)
    train_losses.append(running / len(Xtr))

    model.eval()
    with torch.no_grad():
        out = model(Xva)
        val_losses.append(criterion(out, yva).item())
        val_accs.append((out.argmax(dim=1) == yva).float().mean().item())

    if epoch % 5 == 0 or epoch == 39:
        print(f"epoch {epoch:2d}  train loss {train_losses[-1]:.3f}  "
              f"val loss {val_losses[-1]:.3f}  val acc {val_accs[-1]:.3f}")

model.eval()
with torch.no_grad():
    test_acc = (model(Xte).argmax(dim=1) == yte).float().mean().item()
print("test accuracy:", round(test_acc, 3))
```

Three details to understand. `model.train()` and `model.eval()` switch layers such as dropout and batch normalization between training and evaluation behaviour. Our network has none of those, but the habit is important. `torch.no_grad()` tells PyTorch not to build the computation graph during evaluation, which saves memory and time. And `.item()` converts a one-element tensor into a plain Python number.

## 5. Reading Loss Curves

The curves recorded above are the main diagnostic tool.

```python
import matplotlib.pyplot as plt

plt.plot(train_losses, color="black", label="training loss")
plt.plot(val_losses, color="gray", linestyle="--", label="validation loss")
plt.xlabel("epoch")
plt.ylabel("cross-entropy loss")
plt.legend()
plt.title("Training curves")
plt.show()
```

1. Both training and validation loss fall and then level off together: healthy training.
2. Training loss keeps falling while validation loss turns up: overfitting. The ideas from Week 13 apply directly. Remedies include stopping early, using more data, a smaller network, weight decay or dropout.
3. Loss that does not fall at all, or that jumps wildly: the learning rate is probably too high or too low, or there is a bug in the data or labels.
4. Loss that becomes `nan`: usually a learning rate that is far too large, or a `log(0)` somewhere.

We can provoke the second and third behaviours on purpose. A very large network trained for long may overfit, and an unreasonable learning rate will make training fail.

```python
def train(lr, hidden, epochs=40, seed=0):
    torch.manual_seed(seed)
    m = SimpleNet(64, hidden, 10)
    opt = torch.optim.Adam(m.parameters(), lr=lr)
    for _ in range(epochs):
        for xb, yb in loader:
            opt.zero_grad()
            criterion(m(xb), yb).backward()
            opt.step()
    with torch.no_grad():
        return (criterion(m(Xtr), ytr).item(), criterion(m(Xva), yva).item(),
                (m(Xva).argmax(1) == yva).float().mean().item())

print("lr      hidden  train loss  val loss  val acc")
for lr, hidden in [(1e-3, 32), (1e-5, 32), (1e-1, 32), (1e-3, 256)]:
    tr, va, acc = train(lr, hidden)
    print(f"{lr:<7g} {hidden:6d}  {tr:10.3f}  {va:8.3f}  {acc:7.3f}")
```

A learning rate of 1e-5 is too small: after 40 epochs the loss has barely moved from its starting value of about 2.3 (which is `log(10)`, the loss of guessing among ten classes) and the accuracy is close to chance. A rate of 0.1 is too large for Adam. The training loss falls, but the validation loss is about 1.06 and the accuracy is lower and erratic. The larger network with 256 hidden units reaches a training loss of 0.02 against a validation loss of 0.12. The gap between them has widened, which is the early sign of overfitting, although on this easy dataset the validation accuracy still improves. Watch the gap, and not only the accuracy.

## 6. In-Class Exercise

Train the `SimpleNet` model on a small image dataset, plot the training and validation loss curves, and report test accuracy.

The code in sections 4 and 5 does this on the digits data. For the written part, answer these.

1. After how many epochs does the validation loss stop improving? Would you have stopped earlier?
2. Replace Adam with `torch.optim.SGD(model.parameters(), lr=0.1, momentum=0.9)` and compare the curves.
3. Report the test accuracy and a confusion matrix. Which digits are confused most often?

```python
from sklearn.metrics import confusion_matrix

with torch.no_grad():
    pred = model(Xte).argmax(dim=1).numpy()
cm = confusion_matrix(y_test, pred)
np.fill_diagonal(cm, 0)
worst = np.unravel_index(cm.argmax(), cm.shape)
print("most frequent confusion: true", worst[0], "predicted", worst[1], "count", cm.max())
```

## 7. Common Mistakes

1. Forgetting `optimizer.zero_grad()`, so that gradients pile up.
2. Applying a softmax before `nn.CrossEntropyLoss`.
3. Passing one-hot labels to `CrossEntropyLoss`, which expects class indices.
4. Evaluating without `model.eval()` and `torch.no_grad()`.
5. Looking at the test set during training to decide when to stop.
6. Feeding unscaled inputs, such as pixel values from 0 to 255, which can slow or destabilize training.
7. Comparing runs without fixing the random seed.

## 8. Summary

Backpropagation applies the chain rule backward through the network, reusing intermediate results so that all the gradients are obtained at a cost similar to a forward pass. Optimizers such as SGD, momentum and Adam turn gradients into weight updates, and mini-batches make this cheap. A PyTorch training loop is just forward, loss, backward and step, repeated. The loss curves on the training and validation sets are the way to see whether the training is healthy, and the lessons of Week 13 about overfitting apply here without change.

## 9. Practice Problems

1. Extend the from-scratch network to use softmax with categorical cross-entropy, and train it on the three-class iris data.
2. Add early stopping to the PyTorch loop: stop when the validation loss has not improved for five epochs, and restore the best weights.
3. Add `nn.Dropout(0.3)` after the hidden layer and compare the validation curves with and without it.
4. Use `torch.manual_seed` with five different seeds and report the mean and spread of the test accuracy.

## 10. Suggested Reading

1. The official PyTorch tutorials, "Learn the Basics".
2. Nielsen, Neural Networks and Deep Learning, the chapter on how backpropagation works.
3. Karpathy, "A Recipe for Training Neural Networks", an essay full of practical advice.
