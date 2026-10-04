# Course Contents: Introduction to Artificial Neural Networks

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

---

## Week 1 — Biological Inspiration, History, and Course Roadmap
- **Topics:** The biological neuron (dendrites, soma, axon, synapse) as loose inspiration;
  history of neural networks (McCulloch-Pitts neuron 1943, Hebbian learning, Rosenblatt's
  perceptron 1958, the 1969 Minsky-Papert critique, the 1986 backpropagation popularization, the
  2012 deep learning resurgence); the McCulloch-Pitts neuron model; course roadmap and how this
  course differs from and extends the brief Weeks 14–15 ANN treatment in *Programming for AI*.
- **Subtopics/Skills:** tracing a timeline of key milestones; implementing a McCulloth-Pitts
  binary threshold neuron in Python; setting up the course's NumPy/Matplotlib environment.
- **Readings:** Nielsen Ch. 1 (introduction); Goodfellow et al. Ch. 1 (historical trends).
- **Software:** Python 3.10+, NumPy, Matplotlib.

## Week 2 — The Perceptron and Linear Separability
- **Topics:** The perceptron: weighted sum, bias, step activation; the perceptron learning rule
  (error-driven weight update); geometric interpretation as a separating hyperplane; linear
  separability; why a single perceptron cannot learn XOR.
- **Subtopics/Skills:** implementing a perceptron and its training rule from scratch in NumPy;
  training it on AND/OR (linearly separable) and demonstrating its failure on XOR.
- **Readings:** Nielsen Ch. 1 (perceptrons); Goodfellow et al. Ch. 6.1 (intro to feedforward
  networks, XOR example).
- **Software:** NumPy, Matplotlib.

## Week 3 — Activation Functions in Depth
- **Topics:** Why non-linearity is necessary; sigmoid and its saturation/vanishing-gradient
  properties; tanh; ReLU and why it mitigates vanishing gradients; Leaky ReLU and ELU (brief);
  softmax for multi-class outputs.
- **Subtopics/Skills:** implementing and plotting each activation function and its derivative;
  reasoning about which activation suits a hidden layer vs. an output layer.
- **Readings:** Goodfellow et al. Ch. 6.3 (hidden units); Nielsen Ch. 3 (other activation
  functions, selected sections).
- **Software:** NumPy, Matplotlib.

## Week 4 — The Multi-Layer Perceptron (MLP)
- **Topics:** MLP architecture (input/hidden/output layers); forward propagation in matrix form
  across multiple layers; the Universal Approximation Theorem (conceptual statement, no proof);
  why depth and non-linearity together give expressive power a single perceptron lacks.
- **Subtopics/Skills:** implementing a parameterized, arbitrary-depth forward pass in NumPy using
  weight-matrix/bias lists; verifying output shapes layer by layer.
- **Readings:** Goodfellow et al. Ch. 6.4 (architecture design); Nielsen Ch. 1 (closing sections
  on multi-layer networks).
- **Software:** NumPy.
- **Assignment 1 assigned** (perceptron, activation functions, MLP forward pass).

## Week 5 — Loss Functions
- **Topics:** Mean Squared Error (regression); Binary Cross-Entropy (paired with sigmoid output);
  Categorical Cross-Entropy (paired with softmax output); why the loss function must match the
  task and output activation; a worked derivation of why cross-entropy avoids the sigmoid
  saturation problem that MSE has for classification.
- **Subtopics/Skills:** implementing each loss function and computing it on example
  predictions/labels; reasoning about numerically stable implementations (log-sum-exp trick).
- **Readings:** Goodfellow et al. Ch. 6.2 (output units and cost functions); Nielsen Ch. 3
  (cross-entropy cost function).
- **Software:** NumPy.

## Week 6 — Gradient Descent
- **Topics:** The gradient as the direction of steepest ascent/descent; the gradient descent
  update rule; learning rate and its effect on convergence/divergence; batch gradient descent vs.
  stochastic gradient descent (SGD) vs. mini-batch gradient descent; the importance of shuffling
  data for SGD.
- **Subtopics/Skills:** implementing gradient descent on a simple 1D/2D loss surface (e.g., a
  quadratic bowl) to visualize convergence under different learning rates; implementing
  mini-batching with shuffling.
- **Readings:** Goodfellow et al. Ch. 4.3 (gradient-based optimization), Ch. 8.1 (how learning
  differs from optimization); Nielsen Ch. 1 (gradient descent section).
- **Software:** NumPy, Matplotlib.

## Week 7 — Backpropagation Derivation
- **Topics:** The chain rule for multivariable functions; backpropagation as the repeated
  application of the chain rule, layer by layer, from the loss backward to the input; deriving,
  by hand, the gradients ∂L/∂W and ∂L/∂b for a small 2-layer network with a labeled numeric
  example.
- **Subtopics/Skills:** working the hand-derivation on paper/whiteboard; checking a hand-computed
  gradient against a numerical (finite-difference) gradient approximation.
- **Readings:** Nielsen Ch. 2 (how the backpropagation algorithm works) — the primary reading for
  this week; Goodfellow et al. Ch. 6.5 (back-propagation and other differentiation algorithms).
- **Software:** pencil/paper; NumPy for the finite-difference check.

## Week 8 — Backpropagation From Scratch in NumPy; Midterm Review
- **Topics:** Translating the Week 7 derivation into a full NumPy implementation of forward pass,
  backward pass, and parameter update for a small MLP; a complete worked example trained on a toy
  dataset (e.g., XOR or a small synthetic classification set); review of Weeks 1–7 for the
  midterm.
- **Subtopics/Skills:** implementing a `NeuralNetwork` class with `forward`, `backward`, and
  `update` methods, trained end-to-end without any framework; tracing gradient shapes to ensure
  they match parameter shapes.
- **Readings:** Nielsen Ch. 2 (code walkthrough sections); review notes from Weeks 1–7.
- **Software:** NumPy, Matplotlib.

## Week 9 — Midterm Exam; Training in Practice: Initialization & Vanishing/Exploding Gradients
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: why all-zero (or all-equal) weight
  initialization fails (symmetry problem); small random initialization; Xavier/Glorot and He
  initialization (conceptual, with formulas); an introductory, conceptual look at vanishing and
  exploding gradients in deep networks and why initialization and activation choice both matter.
- **Readings:** Goodfellow et al. Ch. 8.4 (parameter initialization strategies); Nielsen Ch. 5
  (why are deep neural networks hard to train? — vanishing gradient sections).
- **Software:** NumPy.

## Week 10 — Optimizers
- **Topics:** Limitations of plain SGD; SGD with momentum; RMSProp (per-parameter adaptive
  learning rates via a running average of squared gradients); Adam (combining momentum and
  RMSProp with bias correction); brief note on learning rate schedules (step decay, cosine decay).
- **Subtopics/Skills:** implementing momentum, RMSProp, and Adam update rules from scratch in
  NumPy on the Week 8 network; comparing convergence speed/stability across optimizers on the same
  problem.
- **Readings:** Goodfellow et al. Ch. 8.3 (algorithms with adaptive learning rates), Ch. 8.5
  (algorithms with momentum); original Adam paper (Kingma & Ba, 2014) for reference.
- **Software:** NumPy, Matplotlib.
- **Assignment 2 assigned** (backpropagation from scratch).

## Week 11 — Regularization
- **Topics:** Overfitting in neural networks (capacity vs. generalization); L1 and L2 weight
  regularization (weight decay) and their effect on the loss landscape and gradients; dropout
  (training-time random unit deactivation, inverted-dropout scaling); early stopping using a
  validation set.
- **Subtopics/Skills:** adding L2 regularization and dropout to the from-scratch network;
  comparing training/validation curves with and without regularization.
- **Readings:** Goodfellow et al. Ch. 7.1 (parameter norm penalties), Ch. 7.12 (dropout); Nielsen
  Ch. 3 (overfitting and regularization sections).
- **Software:** NumPy, Matplotlib.
- **Capstone project proposal due.**

## Week 12 — Introduction to a Deep Learning Framework
- **Topics:** Automatic differentiation (autograd) as a generalization of the backpropagation
  students implemented by hand; tensors, computational graphs, and `.backward()` (PyTorch) or
  Keras's built-in training loop; building and training an MLP on a real dataset (MNIST or
  Fashion-MNIST) end to end.
- **Subtopics/Skills:** defining a model class/`Sequential` model, a loss function, an optimizer;
  writing a training loop; plotting training/validation loss and accuracy curves.
- **Readings:** Géron Ch. 10 (introduction to artificial neural networks with Keras); PyTorch "60
  Minute Blitz" (autograd and neural networks sections), or Keras Sequential API guide.
- **Software:** PyTorch or TensorFlow/Keras, Matplotlib.
- **Assignment 3 assigned** (optimizers & regularization).

## Week 13 — Convolutional Neural Networks (Basics)
- **Topics:** The convolution operation (kernels/filters, stride, padding); feature maps; pooling
  (max/average); why convolution's weight sharing and local receptive fields suit image data far
  better than a fully-connected MLP; a minimal CNN architecture (conv → pool → conv → pool →
  dense). Depth is intentionally limited here — architectures like ResNet, batch normalization in
  depth, and transfer learning belong to a dedicated Deep Learning course.
- **Subtopics/Skills:** computing a convolution and a max-pool operation by hand on a small matrix;
  building and training a small CNN on MNIST/Fashion-MNIST with the Week 12 framework.
- **Readings:** Goodfellow et al. Ch. 9.1–9.3 (the convolution operation, motivation, pooling);
  Géron Ch. 14 (selected introductory sections).
- **Software:** PyTorch or TensorFlow/Keras.

## Week 14 — Recurrent Neural Networks (Basics)
- **Topics:** Why sequence data (text, time series) needs memory that a plain MLP/CNN lacks; the
  RNN cell (recurrent hidden state update, conceptual and equation-level); unrolling an RNN through
  time; the vanishing-gradient problem in RNNs (conceptual — gradients shrink across many
  time steps); LSTMs introduced conceptually only, as the standard fix (gating mechanism intuition,
  no full gate-equation derivation — full depth belongs to a dedicated Deep Learning course).
- **Subtopics/Skills:** implementing a single RNN cell's forward pass by hand in NumPy for a short
  sequence; building a minimal RNN (or LSTM) for a toy sequence task with the framework.
- **Readings:** Goodfellow et al. Ch. 10.1–10.2, 10.7 (intro to RNNs, LSTM motivation); Nielsen
  does not cover RNNs — supplement with the PyTorch/Keras RNN tutorial.
- **Software:** PyTorch or TensorFlow/Keras.
- **Assignment 4 assigned** (framework, CNN, RNN).

## Week 15 — Evaluating and Debugging Neural Networks
- **Topics:** Reading train/validation/test loss and accuracy curves; diagnosing underfitting
  (both curves high/flat), overfitting (training improves, validation worsens), and a broken
  training loop (loss not decreasing at all — data/label bugs, learning rate too high/low,
  forgotten normalization); hyperparameter tuning basics (learning rate, batch size, hidden
  width/depth, regularization strength) via systematic search.
- **Subtopics/Skills:** given several pre-generated "broken" training runs, diagnosing the fault
  from the loss curve alone; running a small hyperparameter sweep and selecting the best
  configuration by validation performance.
- **Readings:** Goodfellow et al. Ch. 11 (practical methodology, selected sections); Géron Ch. 11
  (training deep neural networks, selected sections).
- **Software:** PyTorch or TensorFlow/Keras, Matplotlib.

## Week 16 — Capstone Presentations, Course Review
- **Topics:** Student capstone project presentations; recap of the course map (perceptron → MLP →
  backpropagation → optimizers/regularization → frameworks → CNN/RNN → evaluation); discussion of
  where this course's foundations lead next (Deep Learning, Machine Learning, and advanced
  architecture courses).
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9 discussion, proposal Week 11, final Week 16)
Students (individually or in pairs) train and evaluate a neural network on a real, small dataset
using a deep learning framework (e.g., an MNIST or Fashion-MNIST digit/image classifier, or a
small tabular-data MLP on a public dataset), and must include a required ablation or comparison
experiment (e.g., with vs. without dropout, or a comparison of two optimizers) that isolates the
effect of one design choice, with results reported honestly and analyzed in a short written
report and a 5–7 minute presentation.
