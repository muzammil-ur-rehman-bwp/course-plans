# Lab Manual 14 — An RNN Cell From Scratch and a Minimal Sequence Model

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Implement a single RNN cell's forward pass by hand in NumPy; build a minimal RNN/LSTM for a toy
sequence task with the framework.

## Setup
Create `lab14.ipynb`.

## Procedure
1. **Task A — RNN cell from scratch:** implement `rnn_cell_forward` and `rnn_forward` as shown
   in lecture; run it on the in-class exercise's sequence and confirm your hidden states match
   the hand-computed values.
2. **Task B — A toy sequence task:** generate a simple synthetic sequence task (e.g., given a
   sequence of 0s and 1s, predict the parity of the running sum so far, or predict the next value
   of a simple repeating pattern); build the dataset as fixed-length sequences.
3. **Task C — Framework model:** build `SeqModel` (or your own) using `nn.RNN` first, train on
   the Task B dataset, and plot the training loss curve.
4. **Task D — LSTM comparison:** repeat Task C using `nn.LSTM` instead of `nn.RNN`, same
   hyperparameters; compare final accuracy/loss between the two, and, if time allows, test both
   on a longer sequence length than used in training to see which degrades less.

## Expected Output
A notebook with Tasks A–D, including the RNN-vs-LSTM comparison.

## Submission
Submit `lab14.ipynb` by the end of the lab session.
