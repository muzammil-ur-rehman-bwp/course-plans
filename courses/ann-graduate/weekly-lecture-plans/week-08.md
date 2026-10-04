# Week 8 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Regularization Theory; Midterm Review

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Compare the classical bias-variance tradeoff against the double descent phenomenon. (*Analyze, Evaluate*)
2. Re-derive weight decay as a Gaussian prior under MAP estimation. (*Apply, Analyze*)
3. Explain dropout as an approximate Bayesian model-averaging procedure. (*Understand, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Optimization theory got us to a minimum — regularization theory asks which minimum generalizes |
| 0:15–0:40 | Bias-variance, classically | The U-shaped test-error-vs-capacity curve; where it comes from |
| 0:40–1:05 | Double descent | The empirical curve that continues past the classical U once capacity exceeds the interpolation threshold; why this complicates the classical story (previewing Weeks 9–10) |
| 1:05–1:15 | Break | — |
| 1:15–1:35 | Weight decay as MAP | $L2$ penalty $\equiv$ a zero-mean Gaussian prior on weights; the regularization strength $\leftrightarrow$ prior variance correspondence |
| 1:35–1:50 | Dropout as Bayesian averaging | Training samples from an implicit ensemble of subnetworks; the inverted-dropout scaling as an approximation to averaging over that ensemble at test time |
| 1:50–2:00 | Midterm review | Weeks 1–8 map; sample question types |

### Materials/Equipment
- Slides: bias-variance curve and double-descent curve, side by side
- Midterm review sheet (topics list, no answers)

### Formative Check (in-class)
Students state, in one sentence each, (a) where the classical bias-variance curve predicts
overfitting should occur for a highly overparameterized network, and (b) what double descent
observes instead.

### Link to Lab/Assessment
Lab 8: Reproduce a small double-descent curve (test error vs. model width) on a controlled
dataset, and compare training/validation curves with and without weight decay and dropout (see
`lab-manuals/lab-08.md`).
