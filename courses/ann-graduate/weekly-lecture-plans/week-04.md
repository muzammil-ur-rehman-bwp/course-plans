# Week 4 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Initialization Theory

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why all-zero and naive unscaled-random initialization fail. (*Understand*)
2. Derive Xavier/Glorot initialization from a variance-preservation argument. (*Apply, Analyze*)
3. Derive He initialization as the analogous argument adjusted for ReLU. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Autodiff computes gradients correctly given *some* weights — but which starting weights make training possible at all? |
| 0:15–0:35 | Why zero/naive init fails | Symmetry breaking; geometric variance decay/explosion across depth |
| 0:35–1:05 | Deriving Xavier/Glorot | Variance-preservation argument for a linear/tanh-like unit; $\mathrm{Var}(W) = 1/n_{in}$ (or the averaged fan-in/fan-out form) |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | Deriving He initialization | Accounting for ReLU zeroing half the pre-activations; $\mathrm{Var}(W) = 2/n_{in}$ |
| 1:45–2:00 | Synthesis | Matching initialization scheme to activation function; what goes wrong if mismatched |

### Materials/Equipment
- Slides: variance-propagation derivation, step by step
- Live-coding environment (Jupyter) for the layer-wise activation-variance experiment

### Formative Check (in-class)
Given a layer with fan-in $n = 100$ and a tanh activation, students compute the Xavier-recommended
weight variance and compare it to the He-recommended variance for the same fan-in, explaining the
factor-of-2 difference.

### Link to Lab/Assessment
Lab 4: Measure layer-wise activation variance across network depth under zero, small-random,
Xavier, and He initialization, and connect the empirical curves to the derived formulas (see
`lab-manuals/lab-04.md`).
