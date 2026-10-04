# Week 8 — Lecture Content: Attention Mechanisms; Midterm Review

## 1. The Information Bottleneck in Basic Seq2Seq

Week 7's decoder saw only the encoder's *final* hidden/cell state — a single fixed-length vector
that must summarize the entire input sequence, however long. For long sequences, this is a severe
bottleneck: information about early tokens can be diluted or lost by the time the encoder reaches
the end of the sequence.

## 2. Attention: a Weighted Combination of All Encoder States

Attention lets the decoder look back at *all* encoder hidden states at every decoding step,
weighted by relevance to what the decoder currently needs. Given encoder hidden states
$\bar{h}_1, \dots, \bar{h}_S$ and a current decoder (query) state $h_t$:

**Step 1 — scores.** Compute a scalar score between the query and each encoder state. Two common
forms:

$$
\text{score}(h_t, \bar{h}_s) = h_t^\top \bar{h}_s \qquad \text{(dot-product)}
$$

$$
\text{score}(h_t, \bar{h}_s) = v^\top \tanh(W_1 h_t + W_2 \bar{h}_s) \qquad \text{(additive / Bahdanau)}
$$

**Step 2 — weights.** Normalize the scores with softmax:

$$
\alpha_{t,s} = \frac{\exp(\text{score}(h_t, \bar{h}_s))}{\sum_{s'=1}^{S} \exp(\text{score}(h_t, \bar{h}_{s'}))}
$$

**Step 3 — context vector.** Take the weighted sum of encoder states:

$$
c_t = \sum_{s=1}^{S} \alpha_{t,s}\, \bar{h}_s
$$

$c_t$ is recomputed at every decoding step, so the decoder can focus on different parts of the
input sequence as it generates each output — there is no single fixed-length bottleneck anymore.

## 3. Implementing Dot-Product Attention

```python
import torch
import torch.nn.functional as F

def dot_product_attention(query, encoder_states):
    # query: (batch, hidden) ; encoder_states: (batch, seq_len, hidden)
    scores = torch.bmm(encoder_states, query.unsqueeze(2)).squeeze(2)   # (batch, seq_len)
    weights = F.softmax(scores, dim=1)                                  # (batch, seq_len)
    context = torch.bmm(weights.unsqueeze(1), encoder_states).squeeze(1)  # (batch, hidden)
    return context, weights
```

```python
# Small worked example: 3 encoder states, 1 query, hidden size 4
encoder_states = torch.randn(1, 3, 4)
query = torch.randn(1, 4)
context, weights = dot_product_attention(query, encoder_states)
print(weights)   # sums to 1 across the 3 positions
print(context.shape)   # (1, 4)
```

## 4. Midterm Review Map (Weeks 1–8)

| Week(s) | Core idea |
|---|---|
| 1 | PyTorch as this course's framework; prerequisites assumed |
| 2 | Initialization, batch norm, dropout, gradient flow — in depth |
| 3–4 | Convolution arithmetic; LeNet → AlexNet → VGG → ResNet |
| 5 | Augmentation, LR schedules/warmup, transfer learning |
| 6 | LSTM/GRU gate equations; why vanilla RNNs struggle |
| 7 | Seq2seq, teacher forcing, exposure bias |
| 8 | Attention: scores → softmax weights → context vector |

## 5. In-Class Exercise

Given three encoder states and one decoder query (small numeric vectors), compute dot-product
attention scores, apply softmax by hand, and compute the resulting context vector.
