# Week 1 — Lecture Content: Graduate ANN Theory Overview

## 1. How This Course Differs From the Prerequisite
*Introduction to Artificial Neural Networks* established, at an applied level: the perceptron and
its learning rule; the multi-layer perceptron (MLP) and matrix-form forward propagation; the
derivation and from-scratch implementation of backpropagation via the chain rule; gradient
descent and its common variants (SGD, momentum, RMSProp, Adam) as black-box update rules; and
basic regularization (L1/L2 penalties, dropout, early stopping). None of this is re-derived here.
This course instead asks, of each of those applied tools, *why* it works, *when* it provably does
not, and *what is still not understood* — a shift from "how do I train a network" to "what, in
precise mathematical terms, makes a network trainable and generalizable at all."

## 2. The Theoretical Landscape (Six Pillars)
1. **Approximation theory** (Week 2): what classes of functions can a network represent at all?
2. **Automatic differentiation** (Week 3): how are the gradients that training needs actually
   computed, in general, beyond the specific layer-by-layer recipe of backpropagation?
3. **Initialization & normalization theory** (Weeks 4–5): why does the *starting point* and the
   *running statistics* of training matter so much for whether training proceeds at all?
4. **Optimization-landscape theory** (Weeks 6–8): what does the loss surface a network trains on
   actually look like, and why do the optimizers from the prerequisite course behave as they do?
5. **Generalization theory** (Weeks 9–11): why does a model with far more parameters than training
   examples often *not* overfit catastrophically, contrary to classical statistical learning
   theory's prediction?
6. **Modern research directions** (Weeks 12–14): three specific, technically grounded current
   theories — the Neural Tangent Kernel, the Lottery Ticket Hypothesis, and information-bottleneck
   perspectives — treated honestly, including what each does *not* establish.

## 3. Explicit Scope Boundaries
This course is about **the neural network itself** — its approximation power, its training
dynamics, and its generalization behavior — considered mostly architecture-agnostically. Two
sibling graduate courses own adjacent territory and are not duplicated here:
- **Deep Learning (Graduate)** owns architecture depth: convolutional networks, recurrent
  networks, Transformers, and generative model architectures, studied as architectures. When this
  course touches an architectural idea (Week 11: skip connections, attention), it is strictly for
  a one-week, theory-only angle (why does this *ease optimization* or *expressivity*), never
  architectural depth.
- **Machine Learning (Graduate)** owns classical statistical ML algorithms (SVMs, ensemble
  methods, etc.), not covered here at all.

## 4. Prerequisite Rapid Review (Compressed)
- A perceptron computes $\hat y = \mathrm{step}(w^\top x + b)$; an MLP stacks affine maps and
  non-linear activations, $a^{(l)} = g(W^{(l)}a^{(l-1)} + b^{(l)})$.
- Backpropagation computes $\partial L/\partial W^{(l)}$ via the recursive rule
  $\delta^{(l)} = (W^{(l+1)\top}\delta^{(l+1)})\odot g'(z^{(l)})$, with
  $\partial L/\partial W^{(l)} = \delta^{(l)}a^{(l-1)\top}$.
- Gradient descent updates $\theta \leftarrow \theta - \alpha\nabla_\theta L$; momentum, RMSProp,
  and Adam modify this with running averages of the gradient and/or its square.
- L2 regularization adds $\frac{\lambda}{2}\|\theta\|^2$ to the loss; dropout randomly zeroes
  units during training.

If any of the above is unfamiliar, revisit *Introduction to Artificial Neural Networks* Weeks
2–11 before Week 2.

## 5. In-Class Exercise
Complete the 5-item written diagnostic (perceptron prediction rule, backprop's recursive $\delta$
rule, the Adam update's two moving averages, the L2-penalty gradient, and dropout's train/eval
distinction) and self-score against the posted key.
