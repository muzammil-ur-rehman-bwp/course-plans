# Week 4 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: The Multi-Layer Perceptron (MLP)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the architecture of a multi-layer perceptron (input, hidden, output layers). (*Understand*)
2. Apply matrix-form forward propagation to compute a network's output across multiple layers.
   (*Apply*)
3. Understand, at a conceptual level, the Universal Approximation Theorem and what it does and
   does not guarantee. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Why a single perceptron fails on XOR; the fix: stack layers |
| 0:15–0:45 | MLP architecture | Input/hidden/output layers; fully-connected weight matrices; notation |
| 0:45–1:15 | Forward propagation in matrix form | $a^{(l)} = g(W^{(l)}a^{(l-1)} + b^{(l)})$, chained across layers |
| 1:15–1:25 | Break | — |
| 1:25–1:45 | Solving XOR with an MLP | Hand-constructed 2-hidden-unit network that solves XOR exactly |
| 1:45–2:00 | Universal Approximation Theorem | Conceptual statement; what it guarantees and what it does not (training is a separate problem) |

### Materials/Equipment
- Live-coding environment, NumPy
- Diagram: 2-input, 2-hidden-unit, 1-output network solving XOR, with weights labeled

### Formative Check (in-class)
Given the hand-constructed XOR-solving network's weights, trace the forward pass by hand for all
four XOR inputs and confirm all four outputs are correct.

### Link to Lab/Assessment
Lab 4: Implement a general, arbitrary-depth forward pass function in NumPy; verify it reproduces
the hand-constructed XOR solution.
