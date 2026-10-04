# Week 5 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Normalization Theory

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Derive Batch Normalization's forward and backward computation. (*Apply, Analyze*)
2. Evaluate the internal-covariate-shift explanation against the loss-landscape-smoothing
   explanation for why BatchNorm works. (*Evaluate*)
3. Explain when Layer Normalization is preferred over Batch Normalization. (*Understand, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Good initialization preserves variance at $t=0$; normalization preserves it *throughout* training |
| 0:15–0:45 | Deriving BatchNorm forward | Per-batch mean/variance normalization; learned scale $\gamma$ and shift $\beta$; train-time vs. eval-time (running statistics) |
| 0:45–1:10 | Deriving BatchNorm backward | Gradient through the normalization statistics (chain rule through mean and variance) |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Why does it work? | Internal covariate shift (original claim) vs. loss-landscape/Lipschitz-smoothing account (better supported) |
| 1:45–2:00 | LayerNorm | Per-example, per-feature normalization; no batch-statistics dependence; why this suits sequence models and small/variable batches |

### Materials/Equipment
- Slides: BatchNorm forward/backward derivation
- Live-coding environment (Jupyter) for a from-scratch BatchNorm implementation

### Formative Check (in-class)
Students explain, in their own words, why BatchNorm's running statistics (not the current batch's
statistics) must be used at evaluation time, and what would go wrong with a batch size of 1 at
training time.

### Link to Lab/Assessment
Lab 5: Implement Batch Normalization's forward and backward pass from scratch in NumPy, implement
Layer Normalization, and compare their behavior as batch size shrinks toward 1 (see
`lab-manuals/lab-05.md`).
