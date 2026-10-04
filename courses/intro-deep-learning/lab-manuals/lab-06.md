# Lab Manual 6 — LSTM and GRU Gate Mechanics

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement the LSTM gate equations by hand for one time step; use `nn.LSTM` and `nn.GRU` on a toy
sequence and compare their behavior.

## Setup
Create `lab06.ipynb`.

## Procedure
1. **Task A — LSTM by hand:** given provided numeric weight matrices/biases and an input/previous
   state, compute $f_t, i_t, o_t, \tilde{c}_t, c_t, h_t$ step by step using plain tensor
   operations (no `nn.LSTMCell`); verify against `nn.LSTMCell` given the same weights.
2. **Task B — GRU by hand:** repeat Task A's approach for the GRU equations ($r_t, z_t,
   \tilde{h}_t, h_t$); verify against `nn.GRUCell`.
3. **Task C — Sequence comparison:** build an `nn.LSTM` and an `nn.GRU` (same hidden size) on a
   provided toy sequence classification/regression task (`lab06_sequences`); train both for the
   same number of epochs and compare final performance and parameter count.
4. **Task D — Long-sequence stress test:** repeat Task C on a longer version of the same task
   (provided as `lab06_sequences_long`) and discuss whether either model's relative performance
   changes as sequence length increases.

## Expected Output
A notebook with Tasks A–D, including the by-hand vs. `nn.LSTMCell`/`nn.GRUCell` verification and
the Task C/D comparison.

## Submission
Submit `lab06.ipynb` by the end of the lab session.
