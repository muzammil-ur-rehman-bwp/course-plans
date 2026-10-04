# Week 8 Lecture Plan — Introduction to Deep Learning
## Topic: Attention Mechanisms; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the information bottleneck problem in basic seq2seq models. (*Understand*)
2. Analyze attention as a learned weighted combination of encoder states. (*Analyze*)
3. Apply the attention score and softmax-weighting computation in PyTorch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | The bottleneck problem | Why a single fixed-length context vector degrades for long sequences |
| 0:15–0:45 | Attention mechanism | Scoring every encoder state against the decoder state; dot-product and additive (Bahdanau) scoring |
| 0:45–1:05 | Softmax weighting & context vector | Converting scores to weights; computing the weighted context vector |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | Live implementation | Dot-product attention score + softmax + context vector, coded step by step |
| 1:45–2:00 | Midterm review | Weeks 1–8 map: deep-net practice, CNNs, sequence models, attention |

### Materials/Equipment
- Live-coding environment, PyTorch
- Midterm review sheet (Weeks 1–8 topic map)

### Formative Check (in-class)
Given three encoder hidden state vectors and one decoder query vector, compute dot-product
attention scores, apply softmax, and compute the resulting context vector by hand.

### Link to Lab/Assessment
Lab 8: Implement attention score computation (dot-product) and softmax weighting in PyTorch on a
provided small set of encoder states; midterm review exercises.
