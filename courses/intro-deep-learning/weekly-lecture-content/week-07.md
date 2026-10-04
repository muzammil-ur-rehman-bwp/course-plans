# Week 7 — Lecture Content: Sequence Models II — Sequence-to-Sequence Architectures

## 1. The Sequence-to-Sequence Architecture

A seq2seq model has two parts:
- An **encoder** (e.g., an LSTM) reads the input sequence and produces a final hidden/cell state
  summarizing it.
- A **decoder** (e.g., another LSTM) is initialized from that final state and generates the output
  sequence, one token/value at a time.

```python
import torch
import torch.nn as nn

class Encoder(nn.Module):
    def __init__(self, input_size, hidden_size):
        super().__init__()
        self.lstm = nn.LSTM(input_size, hidden_size, batch_first=True)

    def forward(self, x):
        _, (h_n, c_n) = self.lstm(x)
        return h_n, c_n

class Decoder(nn.Module):
    def __init__(self, output_size, hidden_size):
        super().__init__()
        self.lstm = nn.LSTM(output_size, hidden_size, batch_first=True)
        self.fc = nn.Linear(hidden_size, output_size)

    def forward(self, y_prev, h, c):
        out, (h, c) = self.lstm(y_prev, (h, c))
        return self.fc(out), h, c
```

## 2. Applications as Seq2Seq Instances

- **Text generation:** input is a prompt/context sequence; output is the continuation, generated
  one token at a time.
- **Time-series forecasting:** input is a window of past observations; output is a sequence of
  future predicted values, generated one step at a time.

Both tasks share the same encoder-decoder structure; only the input/output representation (token
embeddings vs. real-valued observations) and the loss (cross-entropy vs. MSE) differ.

## 3. Teacher Forcing

During training, the decoder is given the *true* previous output token/value as its input at each
step, rather than its own (possibly wrong) prediction. This stabilizes and speeds up training,
because early in training the model's own predictions are mostly noise, and feeding noise back in
would compound errors badly.

```python
def train_step(encoder, decoder, x, y, criterion, optimizer):
    optimizer.zero_grad()
    h, c = encoder(x)
    decoder_input = y[:, :-1, :]     # true previous outputs (teacher forcing)
    target = y[:, 1:, :]
    pred, _, _ = decoder(decoder_input, h, c)
    loss = criterion(pred, target)
    loss.backward()
    optimizer.step()
    return loss.item()
```

At inference time there is no ground truth to feed in, so the decoder must feed back its own
previous prediction (autoregressive generation):

```python
def generate(decoder, h, c, start_token, max_len):
    outputs = []
    decoder_input = start_token          # shape (batch, 1, output_size)
    for _ in range(max_len):
        pred, h, c = decoder(decoder_input, h, c)
        outputs.append(pred)
        decoder_input = pred             # feed the model's own prediction back in
    return torch.cat(outputs, dim=1)
```

This train/inference mismatch is called **exposure bias**: the model never saw its own mistakes
during training, so small errors at inference time can compound across the generated sequence.

## 4. In-Class Exercise

Explain why training with teacher forcing could make a model look good on the training loss curve
yet generate poorly at inference time, and name the concept (from Section 3) responsible.
