# Week 7 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Backpropagation Derivation

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain backpropagation as repeated application of the multivariable chain rule, from the
   loss backward to the input. (*Understand*)
2. Analyze a small 2-layer network's computational graph to derive the gradient of the loss with
   respect to every weight and bias. (*Analyze*)
3. Apply the derived formulas to compute every gradient, by hand, for a labeled numeric example.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | We have a loss gradient rule (Week 6); but $\nabla_\theta L$ for a multi-layer network is not obvious — that is backprop |
| 0:15–0:40 | The chain rule, multivariable | Review with a 2-step scalar composition before going to vectors/matrices |
| 0:40–1:10 | Deriving the output-layer gradient | $\delta^{(L)} = a^{(L)} - y$ for cross-entropy + sigmoid/softmax (from Week 5) |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Deriving the hidden-layer gradient | $\delta^{(l)} = (W^{(l+1)\top}\delta^{(l+1)}) \odot g'(z^{(l)})$; the general recursive rule |
| 1:50–2:00 | Numeric worked example | Walk the full numeric example on a 2-input, 2-hidden, 1-output network |

### Materials/Equipment
- Whiteboard/slides for the chain-rule derivation
- Worked numeric example handout (all intermediate values shown)

### Formative Check (in-class)
Students reproduce the numeric worked example's forward pass and first backward step
($\delta^{(2)}$) independently, checking their own arithmetic against the instructor's.

### Link to Lab/Assessment
Lab 7: Reproduce the hand derivation's numeric example in NumPy and verify every gradient against
a finite-difference approximation.
