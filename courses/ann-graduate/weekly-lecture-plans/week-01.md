# Week 1 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Graduate ANN Theory Overview

**Duration:** 2 hours lecture + 3 hour lab/seminar

### Learning Objectives (Bloom's Level)
1. Recall the assumed undergraduate ANN foundation (perceptron, MLP, backpropagation, basic
   optimizers/regularization) and self-assess readiness for this course. (*Remember*)
2. Explain how this course's theoretical scope differs from, and does not duplicate, the sibling
   graduate Deep Learning and Machine Learning courses. (*Understand*)
3. Map the theoretical landscape of neural network research this course will traverse. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Welcome & course map | Syllabus walkthrough; assessment plan; how this course differs from *Introduction to ANN* |
| 0:15–0:45 | Prerequisite rapid review | Perceptron → MLP → backpropagation, stated as assumed; a compressed recap, not a re-derivation |
| 0:45–1:00 | Break | — |
| 1:00–1:30 | The theoretical landscape | Approximation theory, autodiff, init/normalization theory, optimization-landscape theory, generalization theory, modern research directions — one slide per pillar |
| 1:30–2:00 | Scoping discussion | Explicit boundary: this course vs. Deep Learning (architectures) vs. Machine Learning (classical algorithms) grad courses; where each theory pillar will matter for the capstone |

### Materials/Equipment
- Syllabus and assessment-plan slides
- Prerequisite diagnostic handout (perceptron/MLP/backprop short-answer items)
- Research-landscape map slide

### Formative Check (in-class)
Students complete a short written diagnostic (5 items) on perceptron/MLP/backpropagation basics;
self-score against a posted answer key to identify any prerequisite gaps before Week 2.

### Link to Lab/Assessment
Lab 1: Environment setup and a prerequisite-fluency refresher — reimplement a small two-layer
network's forward and backward pass from scratch, confirming gradients with a finite-difference
check (see `lab-manuals/lab-01.md`).
