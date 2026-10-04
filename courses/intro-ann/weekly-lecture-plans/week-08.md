# Week 8 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Backpropagation From Scratch in NumPy; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply the Week 7 derivation to implement a complete, trainable `NeuralNetwork` class in NumPy.
   (*Apply*)
2. Analyze a full training run (loss curve) to confirm the implementation learns correctly.
   (*Analyze*)
3. Demonstrate recall and application of Weeks 1–7 material in preparation for the midterm.
   (*Remember, Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | From derivation to code | Mapping each Week 7 formula to one line of NumPy |
| 0:20–0:55 | Building the `NeuralNetwork` class | `forward`, `backward`, `update` methods, live-coded |
| 0:55–1:05 | Break | — |
| 1:05–1:30 | Training end-to-end | Training loop on the XOR dataset; watching the loss curve converge |
| 1:30–2:00 | Midterm review | Practice problems spanning Weeks 1–8 (perceptron, activations, MLP, loss, gradient descent, backprop) |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib
- Midterm review problem sheet (Weeks 1–8)

### Formative Check (in-class)
After training the from-scratch network on XOR, each student confirms it reaches near-zero loss
and correctly classifies all four XOR inputs — a direct, concrete resolution of Week 2's
unsolved problem.

### Link to Lab/Assessment
Lab 8: Complete and train the from-scratch `NeuralNetwork` class on XOR and a slightly larger toy
dataset; plot the loss curve.
