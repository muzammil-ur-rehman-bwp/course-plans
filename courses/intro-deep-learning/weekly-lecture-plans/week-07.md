# Week 7 Lecture Plan — Introduction to Deep Learning
## Topic: Sequence Models II — Sequence-to-Sequence Architectures and Applications

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the sequence-to-sequence (encoder-decoder) architecture. (*Understand*)
2. Apply an LSTM-based encoder-decoder to a text-generation or time-series forecasting task in
   PyTorch. (*Apply*)
3. Apply teacher forcing during training and autoregressive generation at inference time. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | LSTM/GRU internals from Week 6 as the building block |
| 0:15–0:40 | The seq2seq architecture | Encoder compresses input into a final hidden/cell state; decoder generates output conditioned on it |
| 0:40–0:55 | Applications | Text generation; time-series forecasting — framed as seq2seq instances |
| 0:55–1:05 | Break | — |
| 1:05–1:30 | Teacher forcing | Why it is used during training; the train/inference mismatch it introduces |
| 1:30–2:00 | Live build | Encoder/decoder LSTM implementation, live-coded end to end on a toy task |

### Materials/Equipment
- Live-coding environment, PyTorch

### Formative Check (in-class)
Explain why using teacher forcing during training but autoregressive generation at inference
time can cause errors to compound during generation ("exposure bias"), in two to three sentences.

### Link to Lab/Assessment
Lab 7: Build and train a small LSTM-based sequence-to-sequence model (text generation or
time-series forecasting) in PyTorch.

### Assessment Note
**Assignment 2** (sequence models) is assigned this week.
