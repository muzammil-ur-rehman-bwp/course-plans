# Week 15 Lecture Plan — Programming for AI
## Topic: Neural Networks II — Backpropagation & Training with PyTorch/Keras

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the intuition behind backpropagation and gradient-based optimization (SGD, Adam). (*Understand*)
2. Apply a deep learning framework (PyTorch or Keras) to build and train a small network. (*Apply*)
3. Analyze training/validation loss curves to assess learning behavior. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:30 | Backpropagation intuition | Chain rule walkthrough on the Week 14 tiny network |
| 0:30–0:55 | Optimizers | SGD, momentum, Adam — conceptual comparison |
| 0:55–1:05 | Break | — |
| 1:05–1:40 | Framework walkthrough | Build a `Sequential`/`nn.Module` model, define loss/optimizer |
| 1:40–2:00 | Training loop | Train on an MNIST-scale dataset; plot loss curves |

### Materials/Equipment
- Live-coding environment, PyTorch or TensorFlow/Keras
- Dataset: MNIST or similarly small image/tabular dataset

### Formative Check (in-class)
Exercise: identify from a loss-curve plot whether training is diverging, converging, or
overfitting.

### Link to Lab/Assessment
Lab 15: Train a small feedforward network end-to-end; report test accuracy. **Assignment 4** due
at the start of this week.
