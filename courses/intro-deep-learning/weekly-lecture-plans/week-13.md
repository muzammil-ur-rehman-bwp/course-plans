# Week 13 Lecture Plan — Introduction to Deep Learning
## Topic: Optimization and Regularization for Deep Nets, Revisited

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Analyze the subtle difference between weight decay and L2 regularization under Adam, and why
   AdamW decouples them. (*Analyze*)
2. Apply label smoothing as a regularizer. (*Apply*)
3. Evaluate mixed-precision and large-batch training considerations conceptually. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | L2 regularization and Adam from the prerequisite course, briefly restated |
| 0:15–0:45 | Weight decay vs. L2 under Adam | Why they coincide under SGD but not under Adam's adaptive scaling; AdamW's decoupled weight decay |
| 0:45–1:00 | Label smoothing | Softened one-hot targets; discouraging overconfidence |
| 1:00–1:10 | Break | — |
| 1:10–1:35 | Mixed-precision training (conceptual) | float16/bfloat16 compute, float32 master weights, loss scaling |
| 1:35–2:00 | Large-batch training | Linear LR scaling with batch size; why warmup (Week 5) matters more here |

### Materials/Equipment
- Live-coding environment, PyTorch

### Formative Check (in-class)
Explain, in two to three sentences, why adding an L2 penalty to the loss before computing Adam's
adaptive per-parameter update does not behave the same as directly decaying the weights after the
update (AdamW).

### Link to Lab/Assessment
Lab 13: Compare `Adam` with an L2 penalty against `AdamW`; implement label smoothing; describe a
mixed-precision training loop conceptually.

### Assessment Note
**Assignment 4** (generative models and practical training) is assigned this week.
