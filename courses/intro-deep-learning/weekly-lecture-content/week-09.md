# Week 9 — Lecture Content: Introduction to Transformers

*Scope note: this week surveys the Transformer architecture at a high level, following Vaswani et
al.'s "Attention Is All You Need" (2017). It is deliberately not a full from-scratch
implementation — that depth belongs to a dedicated advanced/graduate deep learning course.*

## 1. Self-Attention

Week 8's attention connected a decoder query to a separate set of encoder states. **Self-attention**
applies the same idea *within* a single sequence: every position attends to every other position
in that same sequence. Each input vector is projected into three roles via learned weight
matrices $W_Q, W_K, W_V$:

$$
Q = XW_Q, \qquad K = XW_K, \qquad V = XW_V
$$

and the output is the **scaled dot-product attention**:

$$
\text{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V
$$

where $d_k$ is the dimension of each query/key vector. The $\sqrt{d_k}$ scaling keeps the
dot products from growing too large in magnitude as $d_k$ grows, which would otherwise push the
softmax into a very peaked, poorly-conditioned regime.

```python
import torch
import torch.nn.functional as F

def scaled_dot_product_attention(Q, K, V):
    d_k = Q.size(-1)
    scores = torch.matmul(Q, K.transpose(-2, -1)) / (d_k ** 0.5)   # (batch, seq, seq)
    weights = F.softmax(scores, dim=-1)
    return torch.matmul(weights, V), weights

batch, seq_len, d_model = 1, 4, 8
X = torch.randn(batch, seq_len, d_model)
W_Q = torch.randn(d_model, d_model)
W_K = torch.randn(d_model, d_model)
W_V = torch.randn(d_model, d_model)
Q, K, V = X @ W_Q, X @ W_K, X @ W_V
output, weights = scaled_dot_product_attention(Q, K, V)
print(output.shape, weights.shape)   # (1, 4, 8), (1, 4, 4)
```

## 2. Multi-Head Attention (Conceptual)

Rather than computing a single attention over the full $d_{model}$-dimensional vectors,
multi-head attention splits $Q, K, V$ into $h$ smaller "heads," computes scaled dot-product
attention independently in each, and concatenates the results before a final linear projection:

$$
\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W_O
$$

Each head can learn to attend to different kinds of relationships (e.g., one head might track
local syntactic structure while another tracks longer-range dependencies). PyTorch provides this
directly:

```python
mha = torch.nn.MultiheadAttention(embed_dim=8, num_heads=2, batch_first=True)
output, attn_weights = mha(X, X, X)   # self-attention: query=key=value=X
print(output.shape)   # (1, 4, 8)
```

## 3. Positional Encoding

Self-attention computes a weighted sum over all positions with no inherent notion of order — if
the input sequence were permuted, self-attention's computation would permute correspondingly with
no information lost about *which* position is which, but also nothing that says "this token comes
first." **Positional encoding** adds position-dependent values to each input embedding so that
order information is available to the model:

$$
PE_{(pos, 2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{model}}}\right), \qquad
PE_{(pos, 2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$

This is the original sinusoidal scheme from Vaswani et al. (2017); many later Transformer variants
use a learned positional embedding instead, but the purpose — injecting order information — is
the same.

## 4. The Overall Transformer Architecture (Survey)

The Transformer replaces the LSTM/GRU-based encoder and decoder from Weeks 6–8 with stacks built
entirely from self-attention, multi-head attention, and position-wise feed-forward sublayers,
plus residual connections (Week 4) and layer normalization around each sublayer:

- **Encoder stack:** each layer = multi-head self-attention + feed-forward, each with a residual
  connection and normalization.
- **Decoder stack:** each layer = masked multi-head self-attention (a position cannot attend to
  future positions) + multi-head attention over the encoder's output (like Week 8's attention,
  but with queries from the decoder and keys/values from the encoder) + feed-forward.

This is presented here as an architectural map, not a full line-by-line implementation; students
who want to implement a Transformer from scratch will do so in a dedicated follow-on deep learning
course.

## 5. In-Class Exercise

For a 4-token sequence, explain in words what self-attention computes for token 2, and explain
why positional encoding is still necessary even though self-attention already looks at every
token in the sequence.
