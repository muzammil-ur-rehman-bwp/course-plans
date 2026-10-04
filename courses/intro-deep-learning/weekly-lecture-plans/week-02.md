# Week 2 Lecture Plan — Introduction to Deep Learning
## Topic: Deep Networks in Practice — Initialization, Normalization, Gradient Flow

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain Xavier/Glorot and He initialization and the variance-preservation rationale behind
   each. (*Understand*)
2. Apply batch normalization and dropout correctly, including the train/eval mode distinction.
   (*Apply*)
3. Analyze how initialization, normalization, and gradient clipping each mitigate vanishing/
   exploding gradients in deep networks. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & motivation | Why a network "deep enough" breaks the simple initialization/training assumptions of the prerequisite course |
| 0:15–0:40 | Initialization in depth | Symmetry-breaking recap; Xavier/Glorot derivation sketch; He initialization for ReLU |
| 0:40–0:55 | Batch normalization | The algorithm; train-mode batch statistics vs. eval-mode running statistics |
| 0:55–1:05 | Break | — |
| 1:05–1:25 | Dropout revisited | Regularization view vs. implicit ensembling view; inverted dropout recap |
| 1:25–1:50 | Vanishing/exploding gradients revisited | Concrete mitigations: init, batch norm, gradient clipping, skip-connection preview |
| 1:50–2:00 | Synthesis | Which technique addresses which failure mode — a one-slide map |

### Materials/Equipment
- Live-coding environment, PyTorch
- Handout: initialization variance derivations (Xavier, He)

### Formative Check (in-class)
Given a deep MLP that fails to train (loss flat from step 1), identify at least two plausible
causes among {bad initialization, missing normalization, exploding gradients, wrong learning
rate} and state which diagnostic to run first.

### Link to Lab/Assessment
Lab 2: Compare training curves for a deep MLP under different initializations, with and without
batch normalization, and with and without dropout.
