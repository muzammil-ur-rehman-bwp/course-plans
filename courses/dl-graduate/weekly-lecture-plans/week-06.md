# Week 6 Lecture Plan — Deep Learning (Graduate)
## Topic: Advanced Generative Models I — Normalizing Flows and the Diffusion Forward Process

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Derive the change-of-variables formula for densities under an invertible transformation.
   (*Understand*)
2. Implement a simple 1-D normalizing flow and verify its density transformation. (*Apply*)
3. Derive the diffusion forward process and its closed-form marginal. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | VAE/GAN from the undergraduate course; today: two more generative-modeling families |
| 0:15–0:45 | Normalizing flows | Change-of-variables formula; invertibility and the Jacobian determinant; 1-D affine-flow worked example |
| 0:45–1:10 | Live demo | Fitting a 1-D normalizing flow to a bimodal synthetic dataset |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | The diffusion idea & forward process | A Markov chain of Gaussian noising steps; the closed-form marginal $q(x_t\mid x_0)$ |
| 1:45–2:00 | Live demo | Visualizing progressive noising of an image/1-D signal across $t$ |

### Materials/Equipment
- Live-coding environment, PyTorch, Matplotlib
- Slide diagrams: flow density-reshaping illustration; diffusion forward-noising sequence

### Formative Check (in-class)
Given a simple invertible 1-D map $z = f(x) = ax+b$, students derive $p_X(x)$ from a standard
normal $p_Z(z)$ using the change-of-variables formula.

### Link to Lab/Assessment
Lab 6: Implementing a 1-D normalizing flow and the diffusion forward process on toy data (see
`lab-manuals/lab-06.md`).
