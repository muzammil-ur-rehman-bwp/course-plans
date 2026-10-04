# Presentation: Module 1 — Foundations: Approximation & Autodiff (Weeks 1–3)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 1: Foundations: Approximation & Autodiff
2. **Course map** — the six theoretical pillars; how this course differs from the undergraduate
   prerequisite and from the sibling Deep Learning/Machine Learning graduate courses
3. **Prerequisite rapid review** — perceptron → MLP → backprop, one slide, stated as assumed
4. **The Universal Approximation Theorem** — formal statement for single-hidden-layer sigmoidal
   networks
5. **Proof sketch, step 1** — a sigmoid approximates a step function as steepness grows
6. **Proof sketch, steps 2–3** — two steps make a bump; many bumps approximate any continuous
   function
7. **What the theorem does not say** — existence vs. learnability vs. generalization
8. **Depth vs. width** — depth-separation intuition; exponentially-many-units-shallow vs.
   polynomially-many-deep
9. **Computational graphs** — representing a computation as a DAG of elementary operations
10. **Forward-mode AD** — propagating tangents; efficient for few inputs
11. **Reverse-mode AD** — propagating adjoints; efficient for few outputs (the training regime)
12. **Backprop = reverse-mode AD** — mapping the familiar $\delta$-recursion onto the general rule
13. **Module recap** — networks can represent almost anything (Weeks 2); gradients can be computed
    efficiently for any computation graph (Week 3) — onward to *why training actually works*
    (Module 2)

**Speaker notes:** slide 7 (what the theorem does not say) is the hinge of the whole module —
every later pillar (optimization-landscape theory, generalization theory) exists precisely
because approximation theory alone leaves "does training find a good solution" and "does it
generalize" completely open. Do not let students leave Module 1 thinking approximation theory is
a complete account of anything beyond representational capacity.
