# Week 14 — Lecture Content: Recurrent Neural Networks (Basics)

*Scope note: like Week 13's CNN treatment, this week is an intentionally brief, correct
introduction. Full RNN/LSTM/GRU/attention depth belongs to a dedicated Deep Learning course.*

## 1. Why Sequence Data Needs Memory
An MLP or CNN processes a fixed-size input in one pass, with no built-in notion of "what came
before." Sequential data — text, time series, audio — has outputs that depend on the *order* and
*history* of inputs, not just the current one; "the stock went up" means something different
depending on what preceded it. A **Recurrent Neural Network (RNN)** addresses this by maintaining
a **hidden state** that is updated at every time step and carries information forward.

## 2. The RNN Cell
At each time step $t$, given input $x_t$ and the previous hidden state $h_{t-1}$:

$$
h_t = \tanh\bigl(W_{xh} x_t + W_{hh} h_{t-1} + b_h\bigr)
$$

and optionally an output $y_t = W_{hy} h_t + b_y$ at some or all time steps. The *same* weight
matrices $W_{xh}, W_{hh}, W_{hy}$ are reused at every time step (weight sharing across time,
analogous to Week 13's weight sharing across space) — this is what lets an RNN handle sequences
of any length with a fixed number of parameters.

```python
import numpy as np

def tanh(z):
    return np.tanh(z)

def rnn_cell_forward(x_t, h_prev, W_xh, W_hh, b_h):
    z_t = W_xh @ x_t + W_hh @ h_prev + b_h
    h_t = tanh(z_t)
    return h_t

# Unrolling over a 3-step sequence:
def rnn_forward(X_seq, h0, W_xh, W_hh, b_h):
    h = h0
    hidden_states = []
    for x_t in X_seq:          # X_seq: list of input vectors, one per time step
        h = rnn_cell_forward(x_t, h, W_xh, W_hh, b_h)
        hidden_states.append(h)
    return hidden_states
```

## 3. Unrolling Through Time
"Unrolling" an RNN means drawing (or computing) it as a chain: $h_0 \to h_1 \to h_2 \to \dots \to
h_T$, where each arrow applies the *same* cell equation. This chain is exactly analogous to a very
deep feedforward network — depth here is the sequence length $T$, not the number of distinct
layers — and backpropagation through this chain is called **backpropagation through time
(BPTT)**: the same chain-rule recursion from Week 7, applied along the time axis instead of the
layer axis.

## 4. Vanishing Gradients Across Time
Because the same $W_{hh}$ is applied repeatedly, the gradient of a loss at time $T$ with respect
to an early hidden state $h_1$ involves a product of roughly $T$ factors of $W_{hh}^\top$ and
$\tanh'(z_t)$ terms — exactly the same structural cause of vanishing/exploding gradients
discussed in Week 9 for deep feedforward networks, now driven by sequence *length* instead of
network *depth*. In practice, plain ("vanilla") RNNs struggle to learn dependencies spanning more
than a modest number of time steps for exactly this reason.

## 5. LSTMs: A Conceptual Fix
The Long Short-Term Memory (LSTM) cell (Hochreiter & Schmidhuber, 1997) addresses this with a
separate **cell state** $c_t$ that is updated mostly *additively*, gated by learned "forget,"
"input," and "output" gates (each a sigmoid-activated vector in $[0,1]$, computed from $x_t$ and
$h_{t-1}$) that control how much old information to keep, how much new information to add, and
how much of the cell state to expose as the hidden state:

$$
c_t = f_t \odot c_{t-1} + i_t \odot \tilde c_t
$$

The key structural point (full gate-equation derivation is left to a dedicated Deep Learning
course): because $c_t$ is updated by an element-wise, mostly-additive combination rather than
being repeatedly passed through a matrix multiply and a saturating non-linearity at every step,
gradients can flow backward through many time steps with far less shrinkage than in a plain RNN —
the forget gate $f_t$ can, when needed, be learned to stay close to 1, letting gradient information
pass through largely unchanged across a given step.

```python
import torch.nn as nn

class SeqModel(nn.Module):
    def __init__(self, input_size, hidden_size, output_size, cell="lstm"):
        super().__init__()
        RNNClass = nn.LSTM if cell == "lstm" else nn.RNN
        self.rnn = RNNClass(input_size, hidden_size, batch_first=True)
        self.fc = nn.Linear(hidden_size, output_size)

    def forward(self, x):
        out, _ = self.rnn(x)          # out: (batch, seq_len, hidden_size)
        last_step = out[:, -1, :]     # use the final time step's hidden state
        return self.fc(last_step)
```

## 6. In-Class Exercise
For a 1D RNN cell ($W_{xh}=0.5$, $W_{hh}=0.8$, $b_h=0$, $h_0=0$) and input sequence
$x_1=1, x_2=0.5, x_3=-1$, compute $h_1, h_2, h_3$ by hand using $\tanh$, then verify with the
`rnn_forward` code above.
