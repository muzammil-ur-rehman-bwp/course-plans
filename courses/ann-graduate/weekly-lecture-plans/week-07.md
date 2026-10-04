# Week 7 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Optimization Landscape Theory II

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Re-derive Adam's update rule and analyze its known convergence failure cases. (*Analyze, Evaluate*)
2. Explain natural gradient descent conceptually and why it is prohibitively costly in practice.
   (*Understand, Evaluate*)
3. Explain why learning-rate warmup improves early-training stability. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Second-order methods are impractical — adaptive first-order methods are the practical compromise |
| 0:15–0:45 | Adam re-derived | Bias-corrected first/second moment estimates; the role of $\epsilon$ for numerical stability |
| 0:45–1:05 | Adam's failure cases | A simple online convex counterexample on which Adam-style updates fail to converge; what this implies for trusting adaptive optimizers blindly |
| 1:05–1:15 | Break | — |
| 1:15–1:35 | Natural gradient descent | Preconditioning by the Fisher information metric; why this is the "correct" geometry for a probabilistic model; the cost that rules it out at scale |
| 1:35–2:00 | Warmup theory | Why a small initial learning rate prevents large, destabilizing early updates, especially under adaptive optimizers and heavy normalization |

### Materials/Equipment
- Slides: Adam update-rule derivation; the non-convergence counterexample
- Live-coding environment (Jupyter) for the warmup-schedule comparison

### Formative Check (in-class)
Students explain, in one or two sentences, why Adam's second-moment estimate can make the
effective learning rate grow over time in the counterexample shown, and why this is undesirable.

### Link to Lab/Assessment
Lab 7: Implement Adam from scratch, reproduce a small non-convergence example, and implement a
learning-rate warmup+decay schedule, comparing early-training stability with and without warmup
(see `lab-manuals/lab-07.md`).
