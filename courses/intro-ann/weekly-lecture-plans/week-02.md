# Week 2 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: The Perceptron and Linear Separability

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the structure of a perceptron and how it differs from the fixed-weight McCulloch-Pitts
   neuron. (*Understand*)
2. Apply the perceptron learning rule to update weights from misclassified examples, by hand and
   in code. (*Apply*)
3. Analyze why a single perceptron cannot represent a non-linearly-separable function such as
   XOR. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & motivation | From fixed weights (Week 1) to learned weights |
| 0:15–0:40 | The perceptron model | Weighted sum + bias + step activation |
| 0:40–1:10 | The perceptron learning rule | Error-driven weight update; worked numeric example |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Geometric view | Perceptron as a separating hyperplane; linear separability |
| 1:45–2:00 | The XOR problem | Why no single hyperplane separates XOR's classes |

### Materials/Equipment
- Live-coding environment, NumPy
- Plots: AND/OR decision boundaries vs. XOR's non-separable layout

### Formative Check (in-class)
By hand, run two iterations of the perceptron learning rule on a 2-input example and verify the
updated weights correctly classify the training point.

### Link to Lab/Assessment
Lab 2: Implement the perceptron and its learning rule from scratch; train on AND/OR; demonstrate
failure on XOR.
