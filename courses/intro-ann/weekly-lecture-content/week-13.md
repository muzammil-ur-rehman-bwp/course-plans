# Week 13 — Lecture Content: Convolutional Neural Networks (Basics)

*Scope note: this week is an intentionally brief, correct introduction to CNNs. Full architectural
depth (ResNets, batch normalization in depth, transfer learning) belongs to a dedicated Deep
Learning course that builds on this foundation.*

## 1. Why Not Just Flatten the Image for an MLP?
Week 12's MLP flattened each 28×28 MNIST image into a 784-length vector, discarding all
information about which pixels are spatially near each other. Two pixels that are adjacent in the
image and two that are on opposite corners look equally "far apart" to a fully-connected layer.
Convolutional layers instead process small, spatially local neighborhoods with **shared**
weights, directly exploiting the fact that useful image features (edges, textures) are local and
appear at many positions.

## 2. The Convolution Operation
A convolution slides a small **kernel** (filter) of weights across the input, computing a
weighted sum (plus bias) at each position:

$$
(I * K)(i,j) = \sum_{m}\sum_{n} I(i+m,\, j+n)\, K(m,n)
$$

For a 3×3 kernel applied with stride 1 and no padding to a 5×5 input, the output is 3×3 (each
dimension shrinks by `kernel_size - 1`). **Stride** controls how far the kernel moves between
positions (stride 2 skips every other position, halving output size roughly); **padding** (adding
zeros around the input border) can keep the output the same size as the input ("same" padding).

```python
import numpy as np

def conv2d(image, kernel, stride=1):
    kh, kw = kernel.shape
    ih, iw = image.shape
    oh = (ih - kh) // stride + 1
    ow = (iw - kw) // stride + 1
    out = np.zeros((oh, ow))
    for i in range(oh):
        for j in range(ow):
            region = image[i*stride:i*stride+kh, j*stride:j*stride+kw]
            out[i, j] = np.sum(region * kernel)
    return out

image = np.array([[1, 2, 3, 0, 1],
                   [0, 1, 2, 3, 1],
                   [1, 0, 1, 2, 0],
                   [2, 1, 0, 1, 1],
                   [0, 2, 1, 0, 1]], dtype=float)
kernel = np.array([[1, 0, -1],
                     [1, 0, -1],
                     [1, 0, -1]], dtype=float)   # a simple vertical-edge detector
print(conv2d(image, kernel))   # 3x3 output
```

The same kernel (same weights) is applied at every spatial position — **weight sharing** — which
is why a convolutional layer has far fewer parameters than a fully-connected layer processing the
same input, regardless of image size.

## 3. Pooling
Pooling downsamples a feature map by summarizing small regions, most commonly with **max
pooling** (keep the largest value in each region):

```python
def max_pool2d(feature_map, size=2, stride=2):
    h, w = feature_map.shape
    oh = (h - size) // stride + 1
    ow = (w - size) // stride + 1
    out = np.zeros((oh, ow))
    for i in range(oh):
        for j in range(ow):
            region = feature_map[i*stride:i*stride+size, j*stride:j*stride+size]
            out[i, j] = np.max(region)
    return out
```
Pooling reduces the spatial size (and therefore computation) of later layers, and gives a degree
of robustness to small translations of a feature within its pooling window — a feature detected
anywhere in a 2×2 region still produces the same pooled output.

## 4. Why CNNs Suit Image Data
| Property | MLP (flattened input) | CNN |
|---|---|---|
| Parameters for a 28×28 image, one layer | hidden\_units × 784 (grows with image size) | kernel\_size² × channels (independent of image size) |
| Spatial locality | Ignored — all pixels treated as unrelated features | Directly exploited — each unit sees only a local region |
| Same feature at a different position | Must be re-learned by different weights | Automatically detected anywhere, via weight sharing |

This parameter efficiency and built-in locality/translation structure is why CNNs dramatically
outperform MLPs on image tasks, especially as image size grows.

## 5. A Minimal CNN Architecture
```python
import torch.nn as nn

class SmallCNN(nn.Module):
    def __init__(self, num_classes=10):
        super().__init__()
        self.conv1 = nn.Conv2d(1, 8, kernel_size=3, padding=1)   # 1 input channel (grayscale)
        self.conv2 = nn.Conv2d(8, 16, kernel_size=3, padding=1)
        self.pool = nn.MaxPool2d(2, 2)
        self.fc = nn.Linear(16 * 7 * 7, num_classes)             # 28 -> 14 -> 7 after two pools

    def forward(self, x):
        x = self.pool(torch.relu(self.conv1(x)))   # 28x28 -> 14x14
        x = self.pool(torch.relu(self.conv2(x)))   # 14x14 -> 7x7
        x = x.view(x.size(0), -1)                  # flatten only at the very end
        return self.fc(x)
```
Only the final layer is fully connected — the convolutional layers do the spatial feature
extraction, and flattening happens only once, right before the classification layer.

## 6. In-Class Exercise
By hand, compute $(I*K)(0,0)$ and $(I*K)(0,1)$ for the 5×5 image and 3×3 kernel above (stride 1,
no padding), then verify both values against the `conv2d` function's output.
