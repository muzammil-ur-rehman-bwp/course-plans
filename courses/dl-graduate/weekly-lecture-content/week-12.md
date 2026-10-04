# Week 12 — Lecture Content: Large-Scale Training Practices

## 1. Data Parallelism vs. Model Parallelism

- **Data parallelism:** replicate the full model on each of $K$ devices; split each minibatch into
  $K$ shards, one per device; each device computes gradients on its shard; gradients are
  averaged (all-reduced) across devices before the optimizer step. This scales throughput roughly
  linearly in $K$ as long as the model fits on one device and communication overhead stays small
  relative to compute.
- **Model parallelism:** when a single model does not fit in one device's memory, split the
  model itself across devices — e.g., different layers on different devices (**pipeline
  parallelism**) or different slices of a single layer's parameters on different devices
  (**tensor parallelism**). This is needed when model size, not just data throughput, is the
  bottleneck, and it introduces its own communication overhead (activations must cross devices
  mid-forward-pass) and more complex implementation than data parallelism.
Real large-scale training typically **combines both** (e.g., data-parallel replicas, each of
which is itself split via model parallelism).

## 2. Mixed-Precision Training, in Depth

Standard training uses 32-bit floats (FP32) throughout. **Mixed-precision training** performs most
compute (matrix multiplies, convolutions) in a lower-precision format — FP16 (16-bit float, narrow
dynamic range) or BF16 (16-bit float with FP32's exponent range but fewer mantissa bits) — while
keeping numerically sensitive accumulations (e.g., a master copy of weights, or the optimizer's
running statistics) in FP32.

**Why loss scaling is needed (FP16 specifically).** FP16 has a much smaller representable
magnitude range than FP32. Gradients during training are often very small in magnitude; many
legitimately nonzero FP32 gradients underflow to exactly zero once rounded to FP16, silently
discarding gradient signal. **Loss scaling** multiplies the loss by a scale factor $S$ (e.g.,
$2^{14}$) before the backward pass — by linearity of differentiation, every gradient is scaled by
the same factor $S$, shifting small gradients up into FP16's representable range — and divides
the gradients by $S$ again immediately before the optimizer step (after casting back to FP32),
recovering the correctly-scaled update. BF16's wider exponent range (matching FP32's) makes
underflow far less likely, which is why BF16 training often needs no loss scaling at all.

```python
import torch

model = torch.nn.Linear(128, 10).cuda() if torch.cuda.is_available() else torch.nn.Linear(128, 10)
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
scaler = torch.cuda.amp.GradScaler(enabled=torch.cuda.is_available())

def mixed_precision_step(x, y):
    optimizer.zero_grad()
    with torch.autocast(device_type="cuda" if torch.cuda.is_available() else "cpu",
                         dtype=torch.float16, enabled=torch.cuda.is_available()):
        out = model(x)
        loss = torch.nn.functional.cross_entropy(out, y)
    scaler.scale(loss).backward()     # scales the loss before backward (loss scaling)
    scaler.step(optimizer)            # unscales gradients, then steps if no overflow
    scaler.update()                   # adjusts the scale factor for the next iteration
    return loss.item()
```

## 3. Gradient Checkpointing

A standard forward pass stores every layer's activations because the backward pass needs them to
compute local gradients — memory cost grows linearly with depth. **Gradient checkpointing** trades
compute for memory: only a subset of activations ("checkpoints") are stored during the forward
pass; when the backward pass needs an activation that was not stored, the forward computation for
that segment is simply **recomputed** on the fly from the nearest stored checkpoint. This roughly
halves (or further reduces) peak activation memory at the cost of extra forward compute during
the backward pass — valuable when memory, not compute, is the binding constraint (e.g., very deep
networks or long sequences).

```python
import torch
import torch.nn as nn
from torch.utils.checkpoint import checkpoint

class CheckpointedBlock(nn.Module):
    def __init__(self, dim):
        super().__init__()
        self.layer1 = nn.Linear(dim, dim)
        self.layer2 = nn.Linear(dim, dim)

    def _forward_impl(self, x):
        return self.layer2(torch.relu(self.layer1(x)))

    def forward(self, x):
        # recomputes _forward_impl during the backward pass instead of storing its activations
        return checkpoint(self._forward_impl, x, use_reentrant=False)
```

## 4. Efficient Fine-Tuning: Low-Rank Adaptation (LoRA)

Fully fine-tuning a large pretrained weight matrix $W\in\mathbb{R}^{d\times k}$ requires updating
and storing $d\times k$ parameters per adapted layer — expensive to store per-task at scale.
**LoRA** freezes the pretrained $W$ entirely and learns only a **low-rank** update:

$$
W' = W + \Delta W = W + BA, \qquad B\in\mathbb{R}^{d\times r},\ A\in\mathbb{R}^{r\times k},\ r \ll
\min(d,k).
$$

The number of trainable parameters drops from $d\times k$ to $r(d+k)$ — for $r=8$ and
$d=k=1024$, that is $8\times 2048 = 16{,}384$ trainable parameters versus $1{,}048{,}576$ for a
full fine-tune, a roughly 64× reduction. $B$ is typically initialized to zero so that $W'=W$ at
the start of fine-tuning (no behavior change before any training), and $A$ is initialized
randomly.

```python
import torch
import torch.nn as nn

class LoRALinear(nn.Module):
    def __init__(self, in_features, out_features, r=8, alpha=16):
        super().__init__()
        self.frozen = nn.Linear(in_features, out_features)
        self.frozen.weight.requires_grad_(False)
        self.frozen.bias.requires_grad_(False)
        self.A = nn.Parameter(torch.randn(r, in_features) * 0.01)
        self.B = nn.Parameter(torch.zeros(out_features, r))
        self.scaling = alpha / r

    def forward(self, x):
        base = self.frozen(x)
        delta = (x @ self.A.T) @ self.B.T * self.scaling
        return base + delta

layer = LoRALinear(1024, 1024, r=8)
trainable = sum(p.numel() for p in layer.parameters() if p.requires_grad)
total_full_finetune = 1024 * 1024
print(f"LoRA trainable params: {trainable:,}  vs full fine-tune: {total_full_finetune:,}")
```

## 5. In-Class Exercise

For $d=k=768$ and LoRA rank $r=4$, compute the number of trainable LoRA parameters and the
reduction factor relative to a full fine-tune of the same $768\times768$ matrix.
