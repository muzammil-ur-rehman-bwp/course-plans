# Week 3 — Lecture Content: The Transformer Architecture From Scratch

## 1. Scaled Dot-Product Attention, Derived

Given queries $Q \in \mathbb{R}^{n\times d_k}$, keys $K \in \mathbb{R}^{m\times d_k}$, and values
$V \in \mathbb{R}^{m\times d_v}$, attention computes, for each query, a weighted average of the
values, where the weight on value $j$ reflects how well query $i$ "matches" key $j$:

$$
\text{Attention}(Q,K,V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right)V.
$$

**Why the $1/\sqrt{d_k}$ scaling.** If $Q$ and $K$'s entries are i.i.d. with mean 0 and variance 1,
each dot product $q_i\cdot k_j = \sum_{\ell=1}^{d_k} q_{i\ell}k_{j\ell}$ is a sum of $d_k$
independent, zero-mean terms, so $\text{Var}(q_i\cdot k_j) = d_k$ — the raw score's magnitude
grows with $d_k$. Feeding large-magnitude scores into softmax drives it toward a near-one-hot
distribution (softmax saturation), which in turn makes its gradient nearly zero almost everywhere,
stalling learning. Dividing by $\sqrt{d_k}$ renormalizes the score variance back to
$\text{Var}(q_i\cdot k_j/\sqrt{d_k}) = 1$, independent of $d_k$, keeping softmax in a
well-conditioned regime regardless of head dimension.

```python
import torch
import torch.nn.functional as F

def scaled_dot_product_attention(Q, K, V, mask=None):
    d_k = Q.size(-1)
    scores = Q @ K.transpose(-2, -1) / d_k ** 0.5          # (..., n, m)
    if mask is not None:
        scores = scores.masked_fill(mask == 0, float("-inf"))
    weights = F.softmax(scores, dim=-1)                     # (..., n, m)
    return weights @ V, weights                              # (..., n, d_v)
```

## 2. Multi-Head Attention

Rather than computing one attention over the full $d_{model}$-dimensional $Q,K,V$, multi-head
attention projects $Q,K,V$ into $h$ independent, lower-dimensional subspaces, runs scaled
dot-product attention in each subspace ("head") in parallel, and recombines:

$$
\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V), \qquad
\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1,\ldots,\text{head}_h)W^O,
$$

where $W_i^Q, W_i^K \in \mathbb{R}^{d_{model}\times d_k}$, $W_i^V \in \mathbb{R}^{d_{model}\times
d_v}$ (typically $d_k=d_v=d_{model}/h$), and $W^O \in \mathbb{R}^{hd_v\times d_{model}}$. Each head
can specialize in a different kind of relationship (e.g., positional vs. semantic), and the total
parameter/compute cost is comparable to one full-dimensional attention because each head operates
on a proportionally smaller subspace.

```python
import torch.nn as nn

class MultiHeadAttention(nn.Module):
    def __init__(self, d_model, num_heads):
        super().__init__()
        assert d_model % num_heads == 0
        self.h = num_heads
        self.d_k = d_model // num_heads
        self.W_q = nn.Linear(d_model, d_model)
        self.W_k = nn.Linear(d_model, d_model)
        self.W_v = nn.Linear(d_model, d_model)
        self.W_o = nn.Linear(d_model, d_model)

    def forward(self, x_q, x_k, x_v, mask=None):
        B, Nq, _ = x_q.shape
        Nk = x_k.size(1)
        def split_heads(t, N):
            return t.view(B, N, self.h, self.d_k).transpose(1, 2)   # (B, h, N, d_k)
        Q = split_heads(self.W_q(x_q), Nq)
        K = split_heads(self.W_k(x_k), Nk)
        V = split_heads(self.W_v(x_v), Nk)
        out, _ = scaled_dot_product_attention(Q, K, V, mask)        # (B, h, Nq, d_k)
        out = out.transpose(1, 2).contiguous().view(B, Nq, self.h * self.d_k)
        return self.W_o(out)
```

## 3. Sinusoidal Positional Encoding

Self-attention is permutation-equivariant — it has no inherent notion of token order. The original
Transformer injects order information additively, using fixed sinusoids of varying frequency:

$$
PE_{(pos,\,2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{model}}}\right), \qquad
PE_{(pos,\,2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{model}}}\right).
$$

**Why this encodes relative position.** For any fixed offset $k$, $PE_{pos+k}$ is a **linear
function** of $PE_{pos}$ at each frequency: using the angle-addition identities, $\sin(\omega_i
(pos+k)) = \sin(\omega_i pos)\cos(\omega_i k) + \cos(\omega_i pos)\sin(\omega_i k)$ (and similarly
for cosine), where $\omega_i = 10000^{-2i/d_{model}}$. Since $\cos(\omega_i k)$ and
$\sin(\omega_i k)$ depend only on the offset $k$ (not on $pos$), there exists a fixed linear
transformation (rotation) depending only on $k$ that maps $PE_{pos}$ to $PE_{pos+k}$ for every
$pos$ — so the network can, in principle, learn to attend based on relative position via linear
operations on these encodings, even though position is injected additively and absolutely.

```python
import torch

def sinusoidal_positional_encoding(max_len, d_model):
    position = torch.arange(max_len).unsqueeze(1).float()                 # (max_len, 1)
    i = torch.arange(0, d_model, 2).float()
    div_term = torch.pow(10000.0, i / d_model)                            # (d_model/2,)
    pe = torch.zeros(max_len, d_model)
    pe[:, 0::2] = torch.sin(position / div_term)
    pe[:, 1::2] = torch.cos(position / div_term)
    return pe   # (max_len, d_model), add to token embeddings
```

## 4. The Encoder-Decoder Architecture and Layer-Norm Placement

Each **encoder** layer: multi-head self-attention over the input sequence, then a position-wise
feed-forward network, each wrapped in a residual connection and a layer normalization. Each
**decoder** layer additionally includes masked self-attention (causal, Week 4) and
**cross-attention**, where decoder queries attend over the encoder's output keys/values — this is
how the decoder conditions its generation on the encoder's representation of the source sequence.

Two layer-norm placement conventions exist:
- **Post-LN** (original paper): `x = LayerNorm(x + Sublayer(x))` — normalization after the
  residual addition.
- **Pre-LN** (the now-common convention in modern implementations): `x = x +
  Sublayer(LayerNorm(x))` — normalization before the sublayer, so the residual stream itself is
  never renormalized. Pre-LN keeps the residual path's gradient magnitude more stable across many
  stacked layers, which is why most modern deep Transformer implementations default to it despite
  the original paper using post-LN.

```python
class EncoderBlock(nn.Module):
    def __init__(self, d_model, num_heads, d_ff, pre_ln=True):
        super().__init__()
        self.pre_ln = pre_ln
        self.attn = MultiHeadAttention(d_model, num_heads)
        self.ff = nn.Sequential(nn.Linear(d_model, d_ff), nn.ReLU(), nn.Linear(d_ff, d_model))
        self.ln1 = nn.LayerNorm(d_model)
        self.ln2 = nn.LayerNorm(d_model)

    def forward(self, x, mask=None):
        if self.pre_ln:
            x = x + self.attn(self.ln1(x), self.ln1(x), self.ln1(x), mask)
            x = x + self.ff(self.ln2(x))
        else:
            x = self.ln1(x + self.attn(x, x, x, mask))
            x = self.ln2(x + self.ff(x))
        return x
```

## 5. In-Class Exercise

For $d_k = 4$ and a toy query/key pair with dot product $q\cdot k = 16$, compare the softmax
output over two candidate keys (scores $16$ and $4$) with and without the $1/\sqrt{d_k}=0.5$
scaling, and observe how much more peaked (saturated) the unscaled softmax is.
