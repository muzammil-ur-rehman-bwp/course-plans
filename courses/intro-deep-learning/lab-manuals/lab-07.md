# Lab Manual 7 — A Sequence-to-Sequence Model in PyTorch

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Build and train a small LSTM-based encoder-decoder for a text-generation or time-series
forecasting task, using teacher forcing during training and autoregressive generation at
inference.

## Setup
Create `lab07.ipynb`. Choose (or use the provided default) `lab07_text` or `lab07_timeseries`
dataset.

## Procedure
1. **Task A — Encoder/decoder:** implement `Encoder` and `Decoder` classes as in lecture; verify
   output shapes for a sample batch.
2. **Task B — Training with teacher forcing:** implement the training loop using the true
   previous target as decoder input; train for a reasonable number of epochs; plot the training
   loss curve.
3. **Task C — Autoregressive generation:** implement the `generate` function from lecture (no
   teacher forcing); generate output sequences for several validation inputs.
4. **Task D — Qualitative evaluation:** for at least 3 validation examples, compare the generated
   output against the true target and comment on where/why they diverge, connecting any divergence
   to exposure bias where relevant.

## Expected Output
A notebook with Tasks A–D, the training loss curve, and the qualitative comparison in Task D.

## Submission
Submit `lab07.ipynb` by the end of the lab session.
