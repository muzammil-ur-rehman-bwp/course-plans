# Course Contents: Artificial Neural Network (Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Graduate ANN Theory Overview
- **Topics:** Course goals and how this course differs from the undergraduate *Introduction to
  Artificial Neural Networks* (that course taught perceptron → MLP → backpropagation → basic
  optimizers/regularization at an applied level; this course assumes all of that and studies
  neural networks theoretically); rapid review of the assumed prerequisite chain (perceptron →
  MLP → backpropagation), stated as assumed and not re-taught, with a short diagnostic exercise
  instead of re-derivation; the theoretical landscape of neural network research this course will
  cover (approximation theory, automatic differentiation, initialization/normalization theory,
  optimization-landscape theory, generalization theory, and modern research directions); explicit
  scoping against the sibling graduate Deep Learning course (architectures) and graduate Machine
  Learning course (classical algorithms).
- **Subtopics/Skills:** self-assessing prerequisite fluency; setting up the course's NumPy/PyTorch
  environment; reading a short theoretical research landscape map.
- **Readings:** Goodfellow et al. Ch. 6 (review, skim); Nielsen Ch. 1–2 (review, skim).
- **Software:** Python 3.10+, NumPy, PyTorch, Matplotlib.

## Week 2 — Universal Approximation and Expressivity
- **Topics:** The Universal Approximation Theorem for single-hidden-layer feedforward networks
  (statement; proof sketch via sums of localized "bump" functions built from sigmoidal units);
  what the theorem does and does not guarantee (existence of weights, not learnability by
  gradient descent; no bound on required width); depth-versus-width expressivity tradeoffs
  (conceptual — some functions that need exponentially many units in a shallow network can be
  represented with polynomially many units once depth is allowed).
- **Subtopics/Skills:** implementing and visualizing a shallow network approximating a 1D target
  function as hidden width increases; empirically comparing a wide-shallow vs. narrow-deep network
  of comparable parameter count on a function with multiple length scales.
- **Readings:** Goodfellow et al. Ch. 6.4.1 (universal approximation properties and depth).
- **Software:** NumPy, PyTorch, Matplotlib.

## Week 3 — Automatic Differentiation in Depth
- **Topics:** The computational graph as the object automatic differentiation operates on;
  forward-mode AD (propagating tangents alongside values, efficient when inputs are few);
  reverse-mode AD (propagating adjoints backward from a scalar output, efficient when outputs are
  few — the regime neural network training lives in); backpropagation formalized precisely as
  reverse-mode AD applied to the specific graph of a layered neural network.
- **Subtopics/Skills:** implementing a minimal scalar-valued reverse-mode autodiff engine from
  scratch in Python (a computational graph of nodes with `.backward()`); verifying it reproduces
  hand-derived gradients from the prerequisite course.
- **Readings:** Goodfellow et al. Ch. 6.5 (back-propagation and other differentiation algorithms).
- **Software:** Python (pure, no NumPy required for the scalar engine), PyTorch (for comparison
  against `autograd`).
- **Assignment 1 assigned** (Universal Approximation + Automatic Differentiation).

## Week 4 — Initialization Theory
- **Topics:** Signal propagation through a deep linear/near-linear network; why all-zero
  initialization fails (symmetry — every unit in a layer computes the same gradient); why naive
  unscaled random initialization fails (activation variance shrinks or explodes geometrically with
  depth); deriving Xavier/Glorot initialization from a variance-preservation argument for
  tanh/sigmoid-like activations; deriving He initialization from the analogous argument accounting
  for ReLU's zeroing of half its inputs.
- **Subtopics/Skills:** running a layer-wise activation-variance experiment across network depth
  under zero, small-random, Xavier, and He initialization; connecting the empirical variance decay
  to the derived formulas.
- **Readings:** Goodfellow et al. Ch. 8.4 (parameter initialization strategies); Nielsen Ch. 5
  (why are deep networks hard to train — review, extended by this week's derivations).
- **Software:** NumPy, Matplotlib.

## Week 5 — Normalization Theory
- **Topics:** Deriving Batch Normalization's forward computation (per-batch mean/variance
  normalization, learned scale/shift) and backward computation (gradient through the
  normalization statistics); the originally proposed "internal covariate shift" explanation for
  why BatchNorm helps, versus the loss-landscape-smoothing explanation (BatchNorm provably bounds
  the Lipschitz constant of the loss with respect to the normalized activations, a more defensible
  account); Layer Normalization's computation and why it is preferred over BatchNorm for sequence
  models and small/variable batch sizes (no dependence on batch statistics).
- **Subtopics/Skills:** implementing BatchNorm forward and backward from scratch in NumPy;
  implementing LayerNorm and comparing its behavior to BatchNorm as batch size shrinks to 1.
- **Readings:** Goodfellow et al. Ch. 8.7.1 (batch normalization).
- **Software:** NumPy, PyTorch (cross-check against `nn.BatchNorm1d`/`nn.LayerNorm`).

## Week 6 — Optimization Landscape Theory I
- **Topics:** The geometry of neural network loss surfaces in high dimensions: why saddle points
  (not bad local minima) dominate the critical-point landscape as dimensionality grows (conceptual
  argument from random-matrix eigenvalue-sign statistics of the Hessian); first-order methods'
  slow escape from saddle points; Newton's method and the Gauss-Newton approximation as
  theoretically appealing second-order methods (quadratic local convergence); why they are
  impractical at neural-network scale (the Hessian is too large to form or invert).
- **Subtopics/Skills:** computing and inspecting the Hessian eigenvalue spectrum of a small
  network's loss at a found critical point; comparing plain gradient descent's convergence near a
  saddle to Newton's method's on a small, tractable example.
- **Readings:** Goodfellow et al. Ch. 8.2 (challenges in neural network optimization — local
  minima, saddle points, cliffs), Ch. 8.6 (approximate second-order methods).
- **Software:** NumPy, Matplotlib.

## Week 7 — Optimization Landscape Theory II
- **Topics:** Adaptive optimizers revisited rigorously: Adam's update rule re-derived, and its
  known convergence subtleties (Adam can fail to converge even on simple convex online learning
  problems, as shown by counterexamples in the literature that motivated AMSGrad-style fixes); a
  conceptual introduction to natural gradient descent (preconditioning by the Fisher information
  metric rather than the Euclidean metric, and why this is the "right" geometry for a probabilistic
  model, at prohibitive practical cost); learning-rate-schedule theory, specifically why warmup
  (a slow initial learning-rate ramp) improves early-training stability, especially for adaptive
  optimizers and normalization-heavy architectures.
- **Subtopics/Skills:** reproducing a small non-convergence example for Adam-style updates on a
  toy objective; implementing a warmup+decay learning-rate schedule and comparing early-training
  loss stability with and without warmup.
- **Readings:** Goodfellow et al. Ch. 8.5 (algorithms with adaptive learning rates); Kingma & Ba's
  Adam paper and the AMSGrad counterexample paper, both read for their core argument rather than
  cited for exact bibliographic detail.
- **Software:** NumPy, PyTorch.

## Week 8 — Regularization Theory; Midterm Review
- **Topics:** The classical bias-variance tradeoff (test error as a U-shaped curve in model
  capacity) versus the double descent phenomenon observed in overparameterized networks (test
  error can decrease, rise near the interpolation threshold, then decrease again as capacity grows
  further); weight decay (L2 regularization) re-derived as a Gaussian prior on weights under a
  MAP-estimation view; dropout framed as an approximate Bayesian model-averaging procedure
  (training as sampling from an implicit ensemble of subnetworks, conceptual); review of Weeks
  1–8 for the midterm.
- **Subtopics/Skills:** reproducing a small double-descent curve (test error vs. model
  width/capacity) on a controlled synthetic or small real dataset; comparing training/validation
  curves with and without weight decay and dropout.
- **Readings:** Goodfellow et al. Ch. 7.1 (parameter norm penalties), Ch. 7.12 (dropout); review
  notes from Weeks 1–7.
- **Software:** NumPy, PyTorch, Matplotlib.

## Week 9 — Midterm Exam; Generalization Theory I
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: the Probably Approximately Correct (PAC)
  learning framework (a hypothesis class is PAC-learnable if a learner can, with high probability,
  find a hypothesis with low true error from polynomially many samples); VC (Vapnik–Chervonenkis)
  dimension as a measure of hypothesis-class capacity (conceptual definition via shattering, with a
  simple classical example — e.g., the VC dimension of linear threshold classifiers in the plane);
  the classical generalization bound in terms of VC dimension and sample size, and why it predicts
  overfitting for networks with far more parameters than training examples — a prediction
  deep learning regularly violates, motivating the rest of this unit.
- **Readings:** a conceptual treatment of PAC learning and VC dimension (see Goodfellow et al.
  Ch. 5.2, generalization, for the surrounding statistical learning framing); students read one
  short survey-style explanation of VC dimension as part of this week's preparation.
- **Software:** NumPy.

## Week 10 — Generalization Theory II
- **Topics:** Rademacher complexity as a data-dependent, often tighter alternative to VC-dimension
  bounds (conceptual definition: the capacity of a hypothesis class to fit random ±1 labels,
  measured directly on the data distribution rather than worst-case); margin-based generalization
  arguments (a classifier that separates training data with a large margin generalizes better than
  the raw VC bound would suggest, because margin-based complexity measures are smaller); the classic
  empirical observation that motivates modern generalization research — a sufficiently large
  network can fit entirely random labels on a training set to zero training error (demonstrating
  enormous raw capacity) yet, when trained normally on real labels, still generalizes well — showing
  that capacity alone cannot be what classical bounds assume it is.
- **Subtopics/Skills:** empirically estimating a Rademacher-complexity-flavored quantity for a
  small hypothesis class; fitting random labels with a small network and reporting training/test
  error alongside the same network's normal-label performance.
- **Readings:** Goodfellow et al. Ch. 5.2 (capacity, overfitting, underfitting — review and
  extend); a conceptual treatment of Rademacher complexity and margin bounds.
- **Software:** NumPy, PyTorch, Matplotlib.
- **Assignment 2 assigned** (Optimization Landscape + Regularization Theory, covering Weeks 4–8).

## Week 11 — Expressivity and Depth
- **Topics:** A brief, theory-angled look at why residual/skip connections ease optimization in
  very deep networks — the key argument is that a skip connection lets a layer represent an
  identity (or near-identity) map trivially, which keeps gradients from vanishing through many
  stacked layers and smooths the loss landscape near the identity initialization point; this is
  explicitly **not** a treatment of the ResNet architecture itself (that belongs to the Deep
  Learning course), only the optimization-theory argument for why such connections help. Attention
  mechanisms considered briefly from an expressivity viewpoint (a weighted combination over all
  positions gives a layer a more direct, less depth-bottlenecked path for information to flow
  than a purely sequential/convolutional layer) — again a brief theoretical framing, not
  Transformer architecture depth.
- **Subtopics/Skills:** comparing gradient-flow magnitude through a deep plain feedforward network
  versus an equivalent network with skip connections, both near identity initialization; a toy
  attention-weight visualization illustrating the expressivity argument.
- **Readings:** a short conceptual treatment of why identity-mapping shortcuts ease deep-network
  optimization; students are pointed to (but not assigned in architectural depth) the original
  residual-connections paper and an attention-mechanism paper as illustrations, with the
  understanding that full architectural treatment is the Deep Learning course's territory.
- **Software:** NumPy, PyTorch.

## Week 12 — The Neural Tangent Kernel
- **Topics:** The idea, due to Jacot, Gabriel, and Hongler's Neural Tangent Kernel (NTK)
  framework, that in the infinite-width limit, a neural network trained with gradient descent
  behaves like kernel regression against a fixed kernel determined at initialization (the NTK);
  what this reveals (a theoretically tractable account of why sufficiently wide networks train
  easily and can achieve zero training loss via convex-like dynamics in function space) and its
  limits (the kernel is fixed — it does not capture feature learning, which is widely believed to
  matter for the strong generalization of realistic, finite-width networks).
- **Subtopics/Skills:** numerically computing an NTK-flavored kernel for a simple one-hidden-layer
  network at initialization, and comparing predictions from kernel regression against that kernel
  to predictions from actually training the (wide-ish) network with gradient descent on the same
  small dataset.
- **Readings:** students read (at least the abstract, introduction, and main-result statement of)
  Jacot et al.'s Neural Tangent Kernel paper; Goodfellow et al. does not cover NTK — this is a
  primary-literature week.
- **Software:** NumPy, PyTorch.
- **Quiz 5** (Neural Tangent Kernel).

## Week 13 — The Lottery Ticket Hypothesis and Pruning
- **Topics:** Frankle and Carbin's Lottery Ticket Hypothesis: the claim that a randomly-initialized
  dense network contains a sparse subnetwork ("winning ticket") that, trained in isolation from
  that same initialization, can match the full network's accuracy; the iterative magnitude-pruning
  procedure used to find such tickets, and why re-initializing a found sparse mask randomly
  (rather than resetting to its original initialization) typically fails to train as well — the
  evidence for the hypothesis's initialization-dependence claim; model compression and knowledge
  distillation introduced at a basic level as practically-motivated, related techniques for
  getting a smaller network to match a larger one's performance.
- **Subtopics/Skills:** implementing iterative magnitude pruning on a small trained network to find
  a sparse mask; comparing a winning-ticket re-training run (original initialization) against a
  random-reinitialization control at the same sparsity; a minimal knowledge-distillation experiment
  (training a small "student" network against a larger trained "teacher" network's soft outputs).
- **Readings:** students read Frankle & Carbin's Lottery Ticket Hypothesis paper's core
  experimental argument.
- **Software:** PyTorch.
- **Quiz 6** (Lottery Ticket Hypothesis and pruning).

## Week 14 — Information-Theoretic Perspectives
- **Topics:** The information bottleneck idea for understanding deep network training: the
  conjecture that layers progressively compress input information while retaining only what is
  predictive of the label, trading off a "fitting" term (mutual information between a layer's
  representation and the label) against a "compression" term (mutual information between that
  representation and the raw input); why this is presented as a debated, active research area
  rather than settled theory — the empirical mutual-information estimates underlying some of the
  strongest claims have been disputed, and the field has not converged on whether information
  bottleneck dynamics are a cause of good generalization or simply a correlated observation.
- **Subtopics/Skills:** a conceptual, toy-scale exercise estimating binned mutual-information-like
  quantities between a small network's hidden layers and its input/output across training epochs,
  with explicit discussion of the estimator's limitations.
- **Readings:** a conceptual treatment of the information bottleneck idea as applied to deep
  learning, presented alongside its published critiques, without asserting it as settled fact.
- **Software:** NumPy, PyTorch.

## Week 15 — Research Methods and Capstone Work Session
- **Topics:** How to read and critique a theoretical machine learning paper (identifying the
  precise claim, the assumptions it depends on, the strength of the supporting evidence, and its
  limitations); reproducibility in machine learning research (why exact numbers often fail to
  reproduce — seed variance, undocumented hyperparameters, hardware/library-version differences —
  and what a reproducibility-conscious experiment reports, e.g. multiple seeds with spread, not a
  single run); structured in-class work session for the capstone literature review and experiment.
- **Subtopics/Skills:** critiquing a provided short theoretical ML paper excerpt as a guided
  in-class exercise; capstone literature-review and experiment work time with instructor
  consultation.
- **Readings:** none new; students apply the critique framework to their own chosen capstone
  papers.
- **Software:** whatever the student's capstone requires (NumPy/PyTorch).
- **Paper Critique & Presentation due.** **Capstone draft due.**

## Week 16 — Capstone Research Presentations; Course Review
- **Topics:** Student capstone research presentations (conference-style talks); recap of the
  course map (approximation theory → automatic differentiation → initialization/normalization
  theory → optimization-landscape theory → generalization theory → modern research directions);
  discussion of where this course's theory leads next (the sibling Deep Learning and Machine
  Learning graduate courses, and current theoretical ML research venues).
- **Deliverable:** Capstone final paper + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–15 content (per Assessment Plan).

---

## Research Capstone (introduced Week 1 overview, proposal due end of Week 8–9, work session Week
15, final Week 16)
Students (individually or in pairs) choose a neural-network-theory subtopic from this course
(e.g., approximation theory, initialization/normalization theory, optimization-landscape theory,
generalization theory, the Neural Tangent Kernel, the Lottery Ticket Hypothesis, or information
bottleneck perspectives), conduct a literature review of 3–5 papers on that subtopic, carry out a
small reproduced or extended experiment or derivation, and write a short conference-style paper
and deliver a conference-style presentation. Results — including negative or partial results —
must be reported honestly and analyzed soundly; see `assignments/capstone-rubric.md`.
