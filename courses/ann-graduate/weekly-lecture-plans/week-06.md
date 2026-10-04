# Week 6 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Optimization Landscape Theory I

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why saddle points, not bad local minima, dominate high-dimensional neural network loss
   landscapes. (*Understand, Analyze*)
2. Analyze the Hessian eigenvalue structure at a critical point to classify it. (*Analyze*)
3. Evaluate why Newton's method and Gauss-Newton, though theoretically appealing, are impractical
   at neural-network scale. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Normalization smooths the landscape — but what does the landscape look like geometrically? |
| 0:15–0:45 | Critical points and the Hessian | Classifying critical points by Hessian eigenvalue signs; the random-matrix argument for why, in high dimensions, having *all* eigenvalues one sign (a true local min/max) becomes exponentially rare |
| 0:45–1:05 | Saddle points in practice | Why first-order gradient descent slows near saddles but can escape them (unlike true local minima) |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | Newton's method & Gauss-Newton | Quadratic local convergence; the update $\Delta\theta = -H^{-1}\nabla L$; Gauss-Newton as a positive-semidefinite approximation for least-squares-like losses |
| 1:45–2:00 | Why not at scale | $O(p^2)$ storage / $O(p^3)$ inversion cost for $p$ parameters; why this rules out exact second-order methods for real networks |

### Materials/Equipment
- Slides: eigenvalue-sign-combinatorics argument for saddle-point dominance
- Live-coding environment (Jupyter) for small-scale Hessian computation and Newton's method

### Formative Check (in-class)
Given a $2\times 2$ Hessian with eigenvalues of mixed sign, students classify the critical point
and sketch the loss surface's local shape (a saddle).

### Link to Lab/Assessment
Lab 6: Compute the Hessian eigenvalue spectrum of a small network's loss at a found critical
point, and compare gradient descent's convergence near a saddle to Newton's method's on a small,
tractable example (see `lab-manuals/lab-06.md`).
