# Week 2 — Lecture Content: Advanced CNN Architectures

## 1. The ResNet Residual Formulation, in Depth

A plain stack of layers must learn some target mapping $H(x)$ directly. A residual block instead
reparameterizes the target as a **residual** $F(x) = H(x) - x$ and computes

$$
H(x) = F(x) + x,
$$

where $F$ is the stack of convolution/BN/activation layers inside the block and the $+x$ is the
identity shortcut. Two consequences follow directly from this reparameterization:

- **Identity is easy to reach.** If the optimal block mapping is (close to) the identity, $F$ only
  needs to learn (close to) the zero function — achievable with small weights — rather than every
  layer in the stack jointly approximating an identity through nonlinear transformations, which a
  stack of ReLU-containing layers has no simple parameterization for.
- **Gradients have a direct path.** By the multivariate chain rule, $\frac{\partial H}{\partial x}
  = \frac{\partial F}{\partial x} + I$: every residual block contributes an additive identity term
  to the local Jacobian, so gradients flowing backward always have an unobstructed additive path
  through the shortcut, in addition to the (possibly small or vanishing) path through $F$.

*(A full account of why this reshapes the **loss landscape** — e.g., the relationship between
skip connections and the prevalence of saddle points versus poor local minima — is *Artificial
Neural Network*, Graduate's territory; this course uses only the identity-mapping/gradient-path
argument above, which is the architecture-level reason ResNet trains successfully at great depth.)*

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
        return F.relu(out + identity)
```

## 2. DenseNet: Dense Connectivity

Where ResNet adds its input back, DenseNet **concatenates** every preceding layer's output within
a dense block:

$$
x_\ell = H_\ell\big([x_0, x_1, \ldots, x_{\ell-1}]\big),
$$

where $[\cdot]$ denotes channel-wise concatenation and $H_\ell$ is a small composite function
(BN-ReLU-Conv). Each layer adds only a small, fixed number of new feature maps (the **growth
rate** $k$), but every later layer has direct access to every earlier layer's features —
encouraging feature reuse and, like ResNet, giving every layer a short gradient path back to the
block's input (every earlier layer is directly concatenated into every later layer's input, so
its gradient contribution does not have to propagate through every intermediate layer).

```python
import torch
import torch.nn as nn

class DenseLayer(nn.Module):
    def __init__(self, in_channels, growth_rate):
        super().__init__()
        self.bn = nn.BatchNorm2d(in_channels)
        self.conv = nn.Conv2d(in_channels, growth_rate, kernel_size=3, padding=1)

    def forward(self, x):
        out = self.conv(F.relu(self.bn(x)))
        return torch.cat([x, out], dim=1)   # dense connectivity: concatenate, don't add

class DenseBlock(nn.Module):
    def __init__(self, in_channels, growth_rate, num_layers):
        super().__init__()
        layers = []
        channels = in_channels
        for _ in range(num_layers):
            layers.append(DenseLayer(channels, growth_rate))
            channels += growth_rate
        self.block = nn.Sequential(*layers)
        self.out_channels = channels

    def forward(self, x):
        return self.block(x)
```

## 3. Efficiency-Focused Architectures: Depthwise Separable Convolutions

A standard `Conv2d(C_in, C_out, k)` layer costs, per output pixel, $C_{in} \times C_{out} \times
k^2$ multiply-adds, for a total of $H \times W \times C_{in} \times C_{out} \times k^2$. A
**depthwise separable convolution** factors this into two cheaper steps:

1. **Depthwise convolution:** a $k\times k$ filter applied independently to *each* input channel
   (`groups=C_in`), costing $H \times W \times C_{in} \times k^2$ — this step changes spatial
   structure per channel but does not mix channels.
2. **Pointwise convolution:** a $1\times1$ convolution mixing channels, costing $H \times W \times
   C_{in} \times C_{out}$.

Total cost: $H W (C_{in}k^2 + C_{in}C_{out})$, versus the standard convolution's $H W C_{in}
C_{out} k^2$ — a reduction factor of approximately

$$
\frac{1}{C_{out}} + \frac{1}{k^2},
$$

which is substantial for typical $C_{out} \gg k^2$ (e.g., $C_{out}=256, k=3$ gives roughly an
8–9× reduction). This is the core building block behind MobileNet/EfficientNet-style
efficiency-focused designs.

```python
class DepthwiseSeparableConv(nn.Module):
    def __init__(self, in_channels, out_channels, kernel_size=3):
        super().__init__()
        self.depthwise = nn.Conv2d(in_channels, in_channels, kernel_size,
                                    padding=kernel_size // 2, groups=in_channels)
        self.pointwise = nn.Conv2d(in_channels, out_channels, kernel_size=1)

    def forward(self, x):
        return self.pointwise(self.depthwise(x))

# Parameter-count comparison
standard = nn.Conv2d(64, 128, kernel_size=3, padding=1)
sep = DepthwiseSeparableConv(64, 128, kernel_size=3)
std_params = sum(p.numel() for p in standard.parameters())
sep_params = sum(p.numel() for p in sep.parameters())
print(f"standard: {std_params:,}  separable: {sep_params:,}  ratio: {sep_params/std_params:.3f}")
```

## 4. In-Class Exercise

For $C_{in}=32$, $C_{out}=64$, $k=3$: compute the standard convolution's multiply-add count and
the depthwise-separable factorization's multiply-add count by hand, and verify the ratio matches
$\frac{1}{C_{out}}+\frac{1}{k^2}$.
