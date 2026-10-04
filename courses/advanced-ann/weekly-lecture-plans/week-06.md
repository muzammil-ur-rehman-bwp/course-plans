# Week 6 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Sharpness and Generalization

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the flat-vs-sharp-minima intuition and how sharpness is measured in practice.
   (*Understand*)
2. Critically evaluate the sharpness-generalization correlation, including the
   reparameterization critique of its reliability. (*Analyze, Evaluate*)
3. Derive Sharpness-Aware Minimization's update rule from its min-max objective. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 4's margin story and Week 5's feature-learning story both concern *which* minimum is found; sharpness is a third lens on the same question |
| 0:15–0:45 | Flat vs. sharp, measured | Hessian top-eigenvalue/spectral-norm sharpness; perturbation-based proxies |
| 0:45–1:10 | The reliability critique | Reparameterization sensitivity: rescaling a ReLU network changes measured sharpness without changing its function |
| 1:10–1:40 | SAM, derived | The min-max objective; first-order worst-case-perturbation approximation; the two-step practical update |
| 1:40–2:00 | Synthesis | What SAM's empirical success does and does not say about the reparameterization critique |

### Materials/Equipment
- Slides: "Sharpness, Generalization, and SAM"
- Whiteboard for the SAM min-max derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Given a ReLU network with one layer's weights doubled and the next layer's halved (an
exactly function-preserving rescaling), explain why naive Hessian-norm sharpness measured at the
two reparameterizations can differ, and what this implies for using raw sharpness as a
generalization predictor.

### Link to Lab/Assessment
Lab 6: implement a Hessian-top-eigenvalue estimator via power iteration and compare SAM-trained
vs. SGD-trained sharpness and test accuracy (see `lab-manuals/lab-06.md`).
