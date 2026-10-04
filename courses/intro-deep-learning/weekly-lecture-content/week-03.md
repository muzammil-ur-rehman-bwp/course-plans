# Week 3 — Lecture Content: Convolutional Neural Networks I

*Scope note: the prerequisite course mentioned convolution and pooling briefly and conceptually.
This week derives the arithmetic in full depth.*

## 1. The Convolution Operation and Output Size

A convolutional layer slides a kernel of weights across the input, computing a weighted sum (plus
bias) at each position. For a 1D (or single spatial dimension of a 2D) input of size $I$, kernel
size $K$, stride $S$, and padding $P$ (added to both sides), the output size is:

$$
O = \left\lfloor \frac{I + 2P - K}{S} \right\rfloor + 1
$$

For a 2D input, this formula is applied independently to height and width.

```python
def conv_output_size(input_size, kernel_size, stride, padding):
    return (input_size + 2 * padding - kernel_size) // stride + 1

# Example: 32x32 input, 5x5 kernel, stride 1, padding 2 ("same" padding)
print(conv_output_size(32, 5, 1, 2))   # 32 -> output size unchanged
# Example: 32x32 input, 5x5 kernel, stride 1, padding 0
print(conv_output_size(32, 5, 1, 0))   # 28
```

## 2. Receptive Field

The **receptive field** of a unit is the region of the original input that can influence it.
Stacking convolutional layers grows the receptive field: with kernel size $K$ and stride 1 at each
of $L$ stacked layers, the receptive field grows by $(K-1)$ per layer:

$$
R_L = 1 + L \cdot (K - 1) \quad \text{(stride 1 throughout)}
$$

```python
def receptive_field(num_layers, kernel_size):
    return 1 + num_layers * (kernel_size - 1)

print(receptive_field(num_layers=3, kernel_size=3))   # 7: three stacked 3x3 conv layers see a 7x7 region
```

Pooling and stride > 1 grow the receptive field faster still, since each subsequent layer's "pixel"
already summarizes a larger input region.

## 3. Multi-Channel Convolution

A real convolutional layer has $C_{in}$ input channels and $C_{out}$ output channels (filters).
Each output filter has its own $C_{in} \times K \times K$ kernel and one bias:

$$
\text{parameters} = C_{out} \times (C_{in} \times K \times K) + C_{out}
$$

```python
import torch.nn as nn

conv = nn.Conv2d(in_channels=3, out_channels=16, kernel_size=3, stride=1, padding=1)
print(sum(p.numel() for p in conv.parameters()))   # 16*(3*3*3) + 16 = 448
```

## 4. Pooling

Max pooling keeps the largest value in each small region, downsampling the feature map and adding
a degree of robustness to small translations within each pooling window:

```python
pool = nn.MaxPool2d(kernel_size=2, stride=2)   # halves height and width
x = torch.randn(1, 16, 32, 32)
print(pool(x).shape)   # torch.Size([1, 16, 16, 16])
```

## 5. Parameter Sharing: the Exact Comparison

For a 32×32×3 image and a target of 16 output feature maps:

| Layer type | Parameter count |
|---|---|
| Fully-connected (flattened input, 16 hidden units) | $16 \times (32 \times 32 \times 3) + 16 = 49{,}168$ |
| Conv2d(3 → 16, kernel 3×3) | $16 \times (3 \times 3 \times 3) + 16 = 448$ |

The convolutional layer uses roughly **100× fewer parameters** here, and — unlike the
fully-connected layer — this count does not grow if the image is made larger, because the same
kernel is reused (shared) at every spatial position.

```python
import torch.nn as nn

fc = nn.Linear(32 * 32 * 3, 16)
conv = nn.Conv2d(3, 16, kernel_size=3, padding=1)
print(sum(p.numel() for p in fc.parameters()), sum(p.numel() for p in conv.parameters()))
```

## 6. In-Class Exercise

Compute, by hand, the output size and receptive field after two stacked `Conv2d` layers (kernel 5,
stride 1, padding 2) each followed by 2×2 max pooling (stride 2), starting from a 32×32 input.
