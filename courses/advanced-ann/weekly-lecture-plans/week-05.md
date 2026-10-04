# Week 5 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Feature Learning Beyond the Kernel Regime

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Analyze the mechanisms by which finite-width networks escape the NTK/lazy regime. (*Analyze*)
2. Identify concrete empirical signatures distinguishing lazy/kernel-regime training from
   feature-learning-regime training. (*Analyze*)
3. Evaluate why feature learning is believed central to deep learning's empirical success beyond
   what kernel theory can explain. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Weeks 2–3: NTK (frozen kernel) vs. mean-field (distribution can move) as two limits of the same wide network |
| 0:15–0:50 | Escaping the lazy regime | What makes relative parameter movement non-negligible at practical, finite widths; learning-rate scale and output-scale effects |
| 0:50–1:20 | Empirical signatures | Kernel drift over training; hidden-representation task-alignment; width-dependence of the kernel-regression-vs-trained-network gap |
| 1:20–1:50 | Why it matters | The strongest empirical successes of deep learning are widely attributed to representations adapting to data, not to a fixed random-feature kernel; what NTK theory therefore cannot, by its own construction, explain |
| 1:50–2:00 | Synthesis | Recap table: lazy regime vs. feature-learning regime, markers of each |

### Materials/Equipment
- Slides: "Feature Learning: Beyond the Fixed Kernel"
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Given a plot of kernel drift vs. width for a trained network, explain which direction the curve
should trend as width grows, and why a *non-shrinking* drift at very large width would be
surprising given Week 2's theory.

### Link to Lab/Assessment
Lab 5: the course's central empirical comparison — measure kernel drift and the kernel-regression-
vs-trained-network performance gap across a range of widths (see `lab-manuals/lab-05.md`).
