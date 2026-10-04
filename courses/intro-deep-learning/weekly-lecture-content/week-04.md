# Week 4 — Lecture Content: Convolutional Neural Networks II — Architecture Evolution

## 1. LeNet: the Original Pattern

LeNet-5 (LeCun et al.) established the conv → pool → conv → pool → dense pattern still used as a
template today:

```python
import torch.nn as nn

class LeNetStyle(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.net = nn.Sequential(
            nn.Conv2d(1, 6, kernel_size=5, padding=2), nn.ReLU(), nn.MaxPool2d(2, 2),
            nn.Conv2d(6, 16, kernel_size=5), nn.ReLU(), nn.MaxPool2d(2, 2),
            nn.Flatten(),
            nn.Linear(16 * 5 * 5, 120), nn.ReLU(),
            nn.Linear(120, 84), nn.ReLU(),
            nn.Linear(84, num_classes),
        )

    def forward(self, x):
        return self.net(x)
```

## 2. AlexNet and VGG

**AlexNet** scaled this pattern up substantially: more layers, ReLU activations throughout (rather
than tanh/sigmoid), and dropout in the fully-connected layers to control overfitting at a much
larger parameter count. **VGG** standardized the architecture further: instead of varying kernel
sizes, it stacks many small 3×3 convolutions, which was shown to match or exceed the receptive
field of larger kernels while keeping the per-layer design uniform and simple to reason about.

## 3. The Degradation Problem

Naively, stacking more layers should only help — a deeper network can always represent what a
shallower one does, by having extra layers learn the identity function. In practice, very deep
plain (non-residual) networks trained worse as depth increased, not because they lacked capacity,
but because plain stacked non-linear layers struggle to learn even an identity mapping: a stack of
non-linear transformations has no easy way to express "pass the input through unchanged."

## 4. ResNet: Skip Connections

ResNet's residual block reframes what a block of layers must learn. Instead of learning a full
transformation $H(x)$, it learns a residual $F(x) = H(x) - x$, and adds the input back:

$$
H(x) = F(x) + x
$$

Learning $F(x) \approx 0$ (an easy target, e.g. via small weights) now gives an identity mapping
"for free," and the addition gives gradients a direct path back to earlier layers that does not
pass through every intervening non-linearity — directly addressing the vanishing-gradient
concerns from Week 2.

```python
import torch.nn as nn
import torch.nn.functional as F

class ResidualBlock(nn.Module):
    def __init__(self, channels):
        super().__init__()
        self.conv1 = nn.Conv2d(channels, channels, kernel_size=3, padding=1)
        self.bn1 = nn.BatchNorm2d(channels)
        self.conv2 = nn.Conv2d(channels, channels, kernel_size=3, padding=1)
        self.bn2 = nn.BatchNorm2d(channels)

    def forward(self, x):
        identity = x
        out = F.relu(self.bn1(self.conv1(x)))
        out = self.bn2(self.conv2(out))
        out = out + identity        # the skip connection
        return F.relu(out)
```

## 5. A Small CNN with a Residual Block, for CIFAR-10

```python
import torch.nn as nn

class SmallResCNN(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.stem = nn.Sequential(
            nn.Conv2d(3, 32, kernel_size=3, padding=1), nn.BatchNorm2d(32), nn.ReLU(),
        )
        self.res_block = ResidualBlock(32)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc = nn.Linear(32 * 16 * 16, num_classes)   # 32x32 -> 16x16 after one pool

    def forward(self, x):
        x = self.stem(x)
        x = self.res_block(x)
        x = self.pool(x)
        x = x.view(x.size(0), -1)
        return self.fc(x)
```

```python
import torchvision
import torchvision.transforms as T

transform = T.Compose([T.ToTensor()])
train_set = torchvision.datasets.CIFAR10(root="./data", train=True, download=True, transform=transform)
train_loader = torch.utils.data.DataLoader(train_set, batch_size=64, shuffle=True)
```

## 6. In-Class Exercise

Explain why a residual block makes "do nothing" (identity) an easy function to learn, while a
plain stacked block of two convolutions does not have an equally easy path to the same function.
