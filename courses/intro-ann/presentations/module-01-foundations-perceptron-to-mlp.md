# Presentation: Module 1 — Foundations: Perceptron to MLP (Weeks 1–4)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 1: Foundations: Perceptron to MLP
2. **The biological neuron** — loose inspiration diagram
3. **A brief history** — timeline, McCulloch-Pitts (1943) to the 2012 deep learning resurgence
4. **The McCulloch-Pitts neuron** — fixed-weight threshold unit; AND/OR/NOT truth tables
5. **The perceptron** — weighted sum + bias + step activation; the learning rule
6. **Linear separability & XOR** — AND/OR separable; XOR is not — the diagram that motivates
   everything that follows
7. **Activation functions** — sigmoid/tanh/ReLU/Leaky ReLU/softmax plotted side by side, with
   derivatives
8. **Why non-linearity matters** — the linear-composition-collapses-to-linear proof sketch
9. **The multi-layer perceptron** — architecture diagram; matrix-form forward propagation
   ($W^{(l)}a^{(l-1)}+b^{(l)}$)
10. **Solving XOR with an MLP** — the hand-designed 2-hidden-unit network, worked through
11. **The Universal Approximation Theorem** — statement and its two key caveats
12. **Module recap** — perceptron's limitation, concretely resolved; onward to how such networks
    actually *learn* their weights (Module 2)

**Speaker notes:** slide 6 (linear separability & XOR) is the hinge of the entire module — every
later slide either explains why XOR fails (slides 2–7) or how it is fixed (slides 8–10). Do not
rush it.
