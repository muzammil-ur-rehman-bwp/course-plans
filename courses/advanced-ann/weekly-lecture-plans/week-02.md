# Week 2 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: The Neural Tangent Kernel Revisited in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the infinite-width-limit derivation sketch for why training dynamics become
   (approximately) linear in parameters. (*Understand, Analyze*)
2. Name and characterize the "lazy training" regime precisely, in terms of relative parameter
   movement. (*Understand*)
3. Critically evaluate NTK theory's limits as a complete account of deep learning's success.
   (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | One-sentence pointer to the graduate course's NTK result (kernel regression in the infinite-width limit); today we derive *why* |
| 0:15–0:50 | The linearization argument | Taylor-expand f(x;θ) around θ₀; derive the function-space ODE df/dt = −Θ(θ)(f−y) under gradient flow; argue Θ(θ) ≈ Θ(θ₀) stays fixed as width → ∞ |
| 0:50–1:20 | Lazy training, named precisely | Relative parameter movement ‖θ_t−θ₀‖/‖θ₀‖ → 0 as width grows; why NTK-style parameterization produces this automatically |
| 1:20–1:50 | Sharper critique | Fixed kernel ⇒ no feature learning; NTK-regime bounds vs. observed finite-width generalization; empirical kernel drift at practical widths |
| 1:50–2:00 | Synthesis | Recap table: claim, mechanism, what it explains, what it doesn't |

### Materials/Equipment
- Slides: "NTK Revisited: The Infinite-Width Derivation"
- Whiteboard for the function-space-ODE derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Explain, in your own words, why "the kernel Θ(θ) stays fixed during training" and "the network
performs no feature learning" are the same fact stated two ways, in the infinite-width limit.

### Link to Lab/Assessment
Lab 2: compute an empirical NTK at two widths and measure relative parameter movement and kernel
drift during training at each (see `lab-manuals/lab-02.md`).
