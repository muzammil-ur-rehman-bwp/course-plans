# Week 8 — Lecture Content: Quantization and Knowledge Distillation

## 1. Why Quantize
Inference memory and throughput are often bottlenecked by moving weights (and activations) between
memory and compute units, not by raw FLOP count. Storing and computing in a lower-precision
format (e.g., INT8, 1 byte/weight) instead of FP32 (4 bytes/weight) cuts memory footprint roughly
4× and typically increases achievable throughput, at the cost of representing each number less
precisely.

## 2. Post-Training Quantization (PTQ): The Affine Mapping
Given a real-valued tensor $x$ with observed range $[x_{\min}, x_{\max}]$ (estimated via a
calibration pass over representative data) and a target integer range $[0, 2^b-1]$ for bit-width
$b$, the **affine quantization mapping** is:
```
s = (x_max − x_min) / (2^b − 1)                          # scale
z = round( −x_min / s )                                  # zero-point
x_int = clamp( round(x/s) + z,  0,  2^b − 1 )             # quantize
x̂ = s · (x_int − z)                                       # dequantize (the value actually used)
```
$s$ maps the real-valued range onto the integer grid's spacing; $z$ shifts the grid so that
real-valued zero is exactly representable (important for correctly quantizing, e.g., zero-padded
activations). The **quantization error** $x - \hat x$ is bounded by $s/2$ in the worst case (the
rounding operation's maximum error), so smaller $s$ (either a narrower range or a larger $b$)
means less error — but $b$ is fixed by the memory/throughput budget, so for a fixed $b$, the error
is driven entirely by the tensor's dynamic range: a tensor with a few large outlier values forces
a large $s$ (since $s$ is set by $x_{\max}-x_{\min}$), degrading precision for every other
(typically much smaller) value in the tensor — the single most common practical failure mode of
naive PTQ.

```python
import torch

def ptq_quantize(x, bits=8):
    qmax = 2 ** bits - 1
    x_min, x_max = x.min(), x.max()
    scale = (x_max - x_min) / qmax
    zero_point = torch.round(-x_min / scale)
    x_int = torch.clamp(torch.round(x / scale) + zero_point, 0, qmax)
    x_dequant = scale * (x_int - zero_point)
    return x_dequant, scale, zero_point

# Per-channel quantization (one scale/zero-point per output channel) typically reduces error
# substantially versus a single per-tensor scale, precisely because it isolates each channel's
# own dynamic range from outliers in other channels.
def ptq_quantize_per_channel(weight, bits=8, channel_dim=0):
    qmax = 2 ** bits - 1
    dims = [d for d in range(weight.dim()) if d != channel_dim]
    x_min = weight.amin(dim=dims, keepdim=True)
    x_max = weight.amax(dim=dims, keepdim=True)
    scale = (x_max - x_min) / qmax
    zero_point = torch.round(-x_min / scale)
    x_int = torch.clamp(torch.round(weight / scale) + zero_point, 0, qmax)
    return scale * (x_int - zero_point)
```
**The precision/accuracy tradeoff**: lower $b$ means less memory/more throughput but strictly more
quantization error, which propagates through the network's forward pass and generally degrades
task accuracy — the practical question is always how low $b$ can go for a given model/task before
accuracy degrades unacceptably, not whether quantizing degrades accuracy at all (it always does,
to some degree).

## 3. Quantization-Aware Training (QAT), Conceptually
PTQ quantizes an already-trained model's weights with no retraining — fast, but the network's
weights were never optimized with quantization error in mind. **QAT** instead simulates
quantization's rounding error *during* training itself: the forward pass uses the quantized
(fake-quantized, i.e. quantize-then-dequantize) weights/activations, so the loss the network
is trained to minimize already reflects the error quantization will introduce at deployment. The
obstacle is that $\mathrm{round}(\cdot)$ has zero gradient almost everywhere (and is
non-differentiable at integers) — backpropagating through it naively gives no useful learning
signal. The standard fix is the **straight-through estimator (STE)**: in the backward pass,
pretend $\mathrm{round}(\cdot)$ was the identity function (gradient $=1$), so gradients flow
through as if no rounding had happened, while the forward pass still actually rounds. This is a
biased gradient estimator — it does not correspond to differentiating the true forward computation
— but it is empirically effective: the network's weights are nudged, over training, toward values
that are more robust to the rounding error the forward pass keeps re-imposing, typically closing
most of PTQ's accuracy gap at the cost of a full (or partial) retraining pass.

```python
class FakeQuantizeSTE(torch.autograd.Function):
    @staticmethod
    def forward(ctx, x, scale, zero_point, qmax):
        x_int = torch.clamp(torch.round(x / scale) + zero_point, 0, qmax)
        return scale * (x_int - zero_point)

    @staticmethod
    def backward(ctx, grad_output):
        # Straight-through: pass the gradient through unchanged, as if round() were identity.
        return grad_output, None, None, None

fake_quantize = FakeQuantizeSTE.apply
```

## 4. Knowledge Distillation: The Teacher-Student Framework
A large, already-trained **teacher** network $T$ produces logits $z_t$ for an input; a smaller
**student** network $S$ is trained not only against the hard ground-truth label $y$, but also to
match the teacher's own output distribution. The combined **distillation loss**:
```
L = (1 − α) · L_CE(y, σ(z_s))  +  α · T² · D_KL( σ(z_t/T) ‖ σ(z_s/T) )
```
where $\sigma$ is softmax, $z_s$ the student's logits, and $T>1$ a **temperature** that softens
both distributions before the KL term. Why temperature matters: a confident, correctly-trained
teacher's raw softmax output is often close to a one-hot vector, which carries almost no
information beyond the hard label itself; dividing logits by $T>1$ before the softmax spreads
probability mass across the non-top classes, revealing the teacher's *relative* confidence among
them (e.g., a "cat" image's teacher output might, at high temperature, reveal that "dog" is a far
more likely second choice than "truck" — information a one-hot label discards entirely). This
extra signal is informally called **dark knowledge**. The $T^2$ factor rescales the KL term's
gradient magnitude back to the same order as the unscaled cross-entropy term's gradient (since
dividing logits by $T$ shrinks gradients through the softmax by roughly $1/T$ per logit,
$T^2$ compensates so the two loss terms remain comparably weighted as $T$ is varied).

```python
import torch.nn.functional as F

def distillation_loss(student_logits, teacher_logits, labels, alpha=0.5, T=4.0):
    ce = F.cross_entropy(student_logits, labels)
    soft_teacher = F.log_softmax(teacher_logits / T, dim=-1)
    soft_student = F.log_softmax(student_logits / T, dim=-1)
    kd = F.kl_div(soft_student, soft_teacher, log_target=True, reduction="batchmean") * (T ** 2)
    return (1 - alpha) * ce + alpha * kd
```

## 5. Midterm Review (Weeks 1–8)
Structured recap checklist: the VP-SDE and its DDPM discretization; the reverse-time SDE and score
matching; classifier-free guidance's derivation; flow matching's velocity-regression objective;
the MoE gating/routing formulation and load balancing; induction heads vs. the implicit-gradient-
descent analogy (evidence grading); the Bradley-Terry model and the RLHF pipeline; PPO's clipped
objective and the DPO derivation (closed-form optimal policy, implied reward, $Z(x)$
cancellation); PTQ's affine mapping and QAT's straight-through estimator; the distillation loss.
The midterm (Week 9) is qualifying-exam style: expect to be asked to *derive*, not just state,
several of these results.

## 6. In-Class/Lab Exercise
Quantize a small trained network's weight tensors at $b \in \{8,4,2\}$ bits using
`ptq_quantize_per_channel` and measure the resulting task-accuracy drop at each bit-width.
Implement a QAT training loop using `fake_quantize` at $b=4$ and compare its recovered accuracy
against plain PTQ at $b=4$. Train a small student network with `distillation_loss` against a
larger, already-trained teacher on the same toy classification task, and compare the distilled
student's accuracy to the same student architecture trained from hard labels alone.
