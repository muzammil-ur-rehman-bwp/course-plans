# Week 3 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Activation Functions in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why non-linear activation functions are necessary for multi-layer networks to be more
   expressive than a linear model. (*Understand*)
2. Apply sigmoid, tanh, ReLU (and variants), and softmax, and compute their derivatives. (*Apply*)
3. Analyze the trade-offs between activation functions, particularly around gradient saturation.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Why non-linearity matters | Composing linear functions stays linear — a proof sketch |
| 0:15–0:40 | Sigmoid & tanh | Formulas, shapes, derivatives, saturation |
| 0:40–0:50 | Break | — |
| 0:50–1:20 | ReLU and variants | ReLU, Leaky ReLU, ELU (brief); dying-ReLU problem |
| 1:20–1:45 | Softmax | Multi-class probability output; relationship to sigmoid (binary case) |
| 1:45–2:00 | Choosing an activation | Hidden layer vs. output layer guidance table |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib
- Plotted comparison: all activation functions and their derivatives on one figure

### Formative Check (in-class)
Given a network diagram with an unlabeled output layer, decide which activation (sigmoid,
softmax, or none/linear) fits a stated task (binary classification, multi-class classification,
regression) and justify the choice.

### Link to Lab/Assessment
Lab 3: Implement and plot each activation function and its derivative in NumPy.
