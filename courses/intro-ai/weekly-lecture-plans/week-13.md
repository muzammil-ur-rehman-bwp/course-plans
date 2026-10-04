# Week 13 Lecture Plan — Introduction to AI
## Topic: Introduction to Neural Networks (Survey)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the perceptron model and its learning rule. (*Understand*)
2. Apply the perceptron learning rule by hand to update weights on a small example. (*Apply*)
3. Analyze why a single perceptron cannot learn XOR, and explain at a survey level why deeper networks and more compute/data drove the deep learning resurgence. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Biological inspiration | Brief, informal analogy to neurons; immediately note the analogy is loose |
| 0:15–0:45 | The perceptron | Weighted sum, step activation, the perceptron learning rule |
| 0:45–1:00 | Break | — |
| 1:00–1:25 | Worked example | Hand-trace the perceptron learning rule training AND/OR on a few examples |
| 1:25–1:45 | Linear separability & XOR | Why AND/OR are linearly separable and XOR is not; geometric intuition |
| 1:45–2:00 | Why deep learning took off | Multi-layer networks overcome linear separability; brief, honest note on compute/data/architecture advances (no backpropagation derivation) |

### Materials/Equipment
- Slides: perceptron diagram, AND/OR/XOR decision-boundary plots
- Starter notebook: perceptron skeleton (weights, bias, step function)

### Formative Check (in-class)
Exercise: hand-trace two perceptron weight updates on a misclassified AND example; predict (and
then verify) that the perceptron cannot separate XOR.

### Link to Lab/Assessment
Lab 13: Implement a perceptron from scratch in Python; train it on AND/OR; demonstrate its
failure on XOR.
