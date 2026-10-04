# Week 16 — Lecture Content: Capstone Presentations, Course Review

## 1. The Course Map
This course built one continuous argument across 16 weeks:

1. **Weeks 1–2**: a single neuron (McCulloch-Pitts, then the learnable perceptron) can compute
   simple functions but is fundamentally limited to linearly separable problems — it cannot learn
   XOR.
2. **Weeks 3–4**: non-linear activation functions and multiple layers (the MLP) resolve that
   limitation representationally — a small network can compute XOR exactly, and the Universal
   Approximation Theorem says, in principle, far more is representable.
3. **Weeks 5–7**: a correctly matched loss function gives a well-behaved gradient signal, and the
   chain rule — backpropagation — tells us exactly how to compute that gradient with respect to
   every weight in a multi-layer network.
4. **Week 8**: backpropagation becomes real, runnable code — a from-scratch network that
   *learns* to solve XOR, closing the loop opened in Week 2.
5. **Weeks 9–11**: training such networks reliably in practice requires attention to
   initialization, optimizer choice (beyond plain gradient descent), and regularization against
   overfitting.
6. **Week 12**: a deep learning framework automates everything built by hand in Weeks 7–11 via
   autograd, letting training scale to real data.
7. **Weeks 13–14**: two architecture families — CNNs (spatial weight sharing) and RNNs (temporal
   weight sharing) — extend the plain MLP to data with structure an MLP ignores.
8. **Week 15**: a unified evaluation lens — reading loss curves, diagnosing failures, tuning
   hyperparameters — applies across every architecture from Weeks 4–14.

## 2. Where This Leads
Students who want to go deeper than this course's necessarily brief Weeks 13–14 treatment of CNNs
and RNNs should take a dedicated **Deep Learning** course, which covers modern architectures
(ResNets, Transformers/attention, generative models) in the depth this survey could not. Students
interested in the broader statistical/learning-based toolkit beyond neural networks specifically
should take **Machine Learning** (classical algorithms, model selection, broader evaluation
theory). Both build directly on the backpropagation, optimization, and regularization foundations
established here.

## 3. Capstone Presentations
Each student or pair presents their capstone project (5–7 minutes + Q&A), following the required
structure in `presentations/capstone-presentation-template.md`: problem statement, data,
approach, results (including the required ablation/comparison experiment), limitations, and next
steps. Presentations are evaluated per `assignments/capstone-rubric.md`.

## 4. Closing Discussion
With the course map above as a reference, discuss as a class: which single week's idea felt like
the biggest "unlock" in understanding how neural networks actually work, and why? This is meant
as a reflective synthesis exercise, not a graded question.
