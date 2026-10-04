# Week 6 — Lecture Content: Sequence Models I — RNN Limitations, LSTM, GRU

*Scope note: the prerequisite course introduced the RNN cell and the vanishing-gradient problem in
RNNs conceptually, with LSTMs mentioned only as a conceptual fix. This week derives the LSTM and
GRU gate equations in full.*

## 1. Why Vanilla RNNs Struggle With Long Sequences

A vanilla RNN updates its hidden state as:

$$
h_t = \tanh(W_h h_{t-1} + W_x x_t + b)
$$

Backpropagation through time computes $\partial h_T / \partial h_0$ by repeatedly multiplying by
(a function of) $W_h$ across $T$ time steps. If the dominant eigenvalue of $W_h$ (combined with
the derivative of $\tanh$) is consistently less than 1, these repeated products shrink toward zero
— **vanishing gradients** — and the network cannot learn dependencies spanning many time steps.
If it is consistently greater than 1, the products grow without bound — **exploding gradients**.
This is the same repeated-multiplication mechanism behind vanishing/exploding gradients in very
deep feedforward networks (Week 2), applied across time instead of across layers.

## 2. The LSTM Cell

The LSTM introduces a separate **cell state** $c_t$ that is updated mostly additively, plus three
gates that control information flow, each a sigmoid-activated function of the previous hidden
state and current input:

$$
\begin{aligned}
f_t &= \sigma(W_f [h_{t-1}, x_t] + b_f) &&\text{(forget gate)} \\
i_t &= \sigma(W_i [h_{t-1}, x_t] + b_i) &&\text{(input gate)} \\
o_t &= \sigma(W_o [h_{t-1}, x_t] + b_o) &&\text{(output gate)} \\
\tilde{c}_t &= \tanh(W_c [h_{t-1}, x_t] + b_c) &&\text{(candidate cell state)} \\
c_t &= f_t \odot c_{t-1} + i_t \odot \tilde{c}_t &&\text{(cell state update)} \\
h_t &= o_t \odot \tanh(c_t) &&\text{(hidden state)}
\end{aligned}
$$

where $[h_{t-1}, x_t]$ denotes concatenation and $\odot$ is elementwise multiplication. The forget
gate decides how much of the previous cell state to keep; the input gate decides how much of the
new candidate to add; the output gate decides how much of the cell state to expose as the hidden
state. Because $c_t$'s update is a gated sum rather than a repeated matrix multiplication, gradients
can flow back through many time steps largely unimpeded when $f_t \approx 1$ — directly addressing
Section 1's vanishing-gradient mechanism.

```python
import torch

batch, input_size, hidden_size = 4, 10, 20
cell = torch.nn.LSTMCell(input_size, hidden_size)
x_t = torch.randn(batch, input_size)
h_prev = torch.zeros(batch, hidden_size)
c_prev = torch.zeros(batch, hidden_size)
h_t, c_t = cell(x_t, (h_prev, c_prev))   # applies exactly the equations above
```

## 3. The GRU: a Simplified Alternative

The GRU merges the forget and input gates into a single **update gate** $z_t$, and has no separate
cell state:

$$
\begin{aligned}
r_t &= \sigma(W_r [h_{t-1}, x_t] + b_r) &&\text{(reset gate)} \\
z_t &= \sigma(W_z [h_{t-1}, x_t] + b_z) &&\text{(update gate)} \\
\tilde{h}_t &= \tanh(W_h [r_t \odot h_{t-1}, x_t] + b_h) &&\text{(candidate hidden state)} \\
h_t &= (1 - z_t) \odot \tilde{h}_t + z_t \odot h_{t-1} &&\text{(hidden state update)}
\end{aligned}
$$

The reset gate controls how much of the previous hidden state is used when computing the
candidate; the update gate interpolates between the old hidden state and the new candidate — when
$z_t \approx 1$, the GRU simply carries $h_{t-1}$ forward, giving the same kind of near-unimpeded
gradient path as the LSTM's forget gate. With fewer gates and no separate cell state, a GRU has
roughly 25% fewer parameters than an LSTM of the same hidden size.

```python
gru_cell = torch.nn.GRUCell(input_size, hidden_size)
h_t = gru_cell(x_t, h_prev)
```

## 4. Using `nn.LSTM`/`nn.GRU` Over a Full Sequence

```python
lstm = torch.nn.LSTM(input_size=10, hidden_size=20, batch_first=True)
x = torch.randn(4, 7, 10)             # (batch, seq_len, input_size) since batch_first=True
output, (h_n, c_n) = lstm(x)
print(output.shape, h_n.shape, c_n.shape)
# output: (4, 7, 20) — hidden state at every time step
# h_n, c_n: (1, 4, 20) — final hidden/cell state (num_layers=1)
```

## 5. In-Class Exercise

Given $f_t = 0.1$ and $i_t = 0.9$ at some time step, explain qualitatively what happens to $c_t$:
does the cell mostly forget its previous content, mostly keep it, or something else?
