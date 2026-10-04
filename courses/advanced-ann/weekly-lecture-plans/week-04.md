# Week 4 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Implicit Regularization and the Implicit Bias of Gradient Descent

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the implicit-regularization question precisely: why gradient descent selects one
   particular global minimum among many. (*Understand*)
2. Derive the max-margin implicit bias of gradient descent on linearly separable data. (*Apply,
   Analyze*)
3. Critically survey implicit-bias research in nonlinear/deep settings as a much harder, only
   partially understood extension. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Many global minima fit separable training data perfectly; which one does GD find, and why does it matter? |
| 0:15–0:55 | The max-margin derivation | Exponential-tail loss argument; norm growth and direction convergence; the hard-margin-SVM limit |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | Nonlinear/deep survey | Matrix factorization and deep-linear-network bias-toward-simplicity results; why the general nonlinear case resists this style of proof |
| 1:35–2:00 | Synthesis | What is settled (linear case) vs. open (general deep nonlinear case) |

### Materials/Equipment
- Slides: "Implicit Bias: From Max-Margin to Deep Networks"
- Whiteboard for the exponential-tail/norm-growth derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Explain why the logistic loss's exponential tail, rather than its exact functional form, is the
property driving the max-margin result, and predict (without re-deriving) whether a loss with a
heavier (polynomial) tail would produce the same bias.

### Link to Lab/Assessment
Lab 4: run gradient descent on separable synthetic data and track the normalized weight vector's
convergence to the max-margin direction found by an exact solver (see `lab-manuals/lab-04.md`).
