# Lab Manual 8 — Attention Score Computation; Midterm Review

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement dot-product attention score computation and softmax weighting in PyTorch; complete
midterm review exercises covering Weeks 1–8.

## Setup
Create `lab08.ipynb`.

## Procedure
1. **Task A — Attention from scratch:** implement `dot_product_attention(query,
   encoder_states)` as in lecture; verify, for a provided small example, that the returned weights
   sum to 1 and that the context vector is a valid weighted combination.
2. **Task B — Attention over Lab 7's model:** add a dot-product attention layer to Lab 7's
   decoder (compute attention over all encoder outputs at each decoding step, not just the final
   state); retrain and compare performance against the no-attention version.
3. **Task C — Midterm review set:** work through the provided review problem set covering Weeks
   1–8 (initialization/normalization, convolution arithmetic, CNN architectures, LSTM/GRU
   equations, seq2seq, attention).
4. **Task D — Self-assessment:** identify, in a short markdown cell, the two topics from Weeks
   1–8 you feel least confident about, and what you plan to review before the midterm.

## Expected Output
A notebook with Tasks A–D, including the Task B comparison and the Task C review answers.

## Submission
Submit `lab08.ipynb` by the end of the lab session.
