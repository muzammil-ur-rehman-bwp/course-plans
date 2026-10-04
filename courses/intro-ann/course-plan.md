# Course Plan: Introduction to Artificial Neural Networks

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Introduction to Artificial Neural Networks |
| Level | Undergraduate (3rd/4th year, BS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | Python programming (equivalent to *Programming for Artificial Intelligence* or a standalone Python course); basic linear algebra (vectors, matrices, matrix multiplication) and calculus (derivatives, the chain rule); basic probability & statistics (random variables, expectation, basic distributions) |
| Programming Language | Python 3.x |
| Core Libraries | NumPy (from-scratch implementations, Weeks 1–11), Matplotlib (visualization throughout), PyTorch or TensorFlow/Keras (Weeks 12–16) |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab (hands-on, Jupyter/Colab based) |

## 2. Course Description

This course is a full semester dedicated entirely to the theory and practice of artificial neural
networks. Where *Programming for Artificial Intelligence* gives neural networks a brief,
two-week, applied introduction (perceptron, forward pass, and a framework-based training loop) as
one topic among many, this course starts from that same starting point — the single perceptron —
and goes deep: it derives backpropagation by hand and implements it from scratch in NumPy, studies
gradient-based optimization (SGD, momentum, RMSProp, Adam) and regularization (L1/L2, dropout,
early stopping) in mathematical detail, and only then introduces a modern deep learning framework
(PyTorch or Keras) for practical training. The course closes with a conceptual introduction to the
two architecture families that extend the plain multi-layer perceptron — convolutional networks
for spatial data and recurrent networks for sequential data — pitched at a depth appropriate for an
undergraduate survey, with the expectation that students who want the full depth of these
architectures will take a dedicated Deep Learning course afterward. Every week of lecture is
paired with a hands-on lab, and the course culminates in a capstone project in which students train
and evaluate a neural network on a real dataset using a modern framework.

## 3. Goals

- Build a precise, from-first-principles understanding of what a neural network computes and why
  it can learn: the perceptron, activation functions, and the multi-layer perceptron.
- Derive and implement backpropagation by hand, so that framework automatic differentiation is
  understood as a mechanization of a well-understood algorithm, not a black box.
- Understand the mathematics and practical behavior of the gradient-based optimizers and
  regularization techniques that make deep networks trainable in practice.
- Gain working fluency with a modern deep learning framework (PyTorch or Keras) for building,
  training, and evaluating networks on real data.
- Acquire a correct conceptual foundation for convolutional and recurrent architectures, sufficient
  to read introductory deep learning material and to take a follow-on Deep Learning course.
- Design, train, and critically evaluate an original neural network model as a capstone project,
  including a required ablation or comparison experiment.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the biological inspiration and history of neural networks, and explain the structure and limitations of a single perceptron. | Remember, Understand |
| CLO2 | Apply activation functions, loss functions, and matrix-form forward propagation to compute the output of a multi-layer perceptron by hand and in NumPy. | Apply |
| CLO3 | Derive, by applying the chain rule layer by layer, the backpropagation gradients of a small network, and implement backpropagation from scratch in NumPy. | Analyze, Apply |
| CLO4 | Analyze the behavior of gradient descent, weight initialization schemes, and modern optimizers (momentum, RMSProp, Adam), and select appropriate regularization techniques to control overfitting. | Analyze, Evaluate |
| CLO5 | Build, train, and evaluate feedforward, convolutional, and recurrent neural networks using a deep learning framework (PyTorch or Keras). | Apply, Analyze |
| CLO6 | Evaluate a trained network's behavior from training/validation curves, diagnose common training failures, and tune hyperparameters accordingly. | Analyze, Evaluate |
| CLO7 | Design, train, and present an original neural network project on a real dataset, including a justified ablation or comparison experiment. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Foundations | 1–4 | Remember, Understand, Apply | Biological inspiration, perceptron, activation functions, the MLP |
| Learning Theory | 5–8 | Apply, Analyze | Loss functions, gradient descent, backpropagation derivation and from-scratch implementation |
| Training in Practice | 9–11 | Analyze, Evaluate | Initialization, vanishing/exploding gradients, optimizers, regularization |
| Frameworks & Architectures | 12–14 | Apply, Analyze | Autograd-based training, CNNs, RNNs |
| Evaluation & Synthesis | 15–16 | Evaluate, Create | Debugging and tuning networks; capstone project |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Biological inspiration & history of neural networks; the McCulloch-Pitts neuron; course roadmap | Remember, Understand |
| 2 | The perceptron: weighted sum, step activation, the perceptron learning rule; linear separability and XOR | Understand, Apply |
| 3 | Activation functions in depth: sigmoid, tanh, ReLU and variants, softmax; why non-linearity matters | Understand, Apply |
| 4 | The multi-layer perceptron: architecture, forward propagation in matrix form, universal approximation | Understand, Apply |
| 5 | Loss functions: MSE, cross-entropy; matching the loss to the task and output activation | Apply, Analyze |
| 6 | Gradient descent: the gradient, learning rate, batch/stochastic/mini-batch variants | Apply, Analyze |
| 7 | Backpropagation derivation: the chain rule applied layer by layer, by hand on a small network | Analyze |
| 8 | Implementing backpropagation from scratch in NumPy; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Training in practice: weight initialization, vanishing/exploding gradients | Remember–Analyze |
| 10 | Optimizers: momentum, RMSProp, Adam; learning rate schedules | Apply, Analyze |
| 11 | Regularization: overfitting, L1/L2, dropout, early stopping | Apply, Analyze |
| 12 | Introduction to a deep learning framework: autograd, training an MLP on real data (MNIST) | Apply |
| 13 | Convolutional Neural Networks: convolution, pooling, why CNNs suit image data | Understand, Apply |
| 14 | Recurrent Neural Networks: the RNN cell, vanishing gradients in RNNs, LSTMs as a conceptual fix | Understand |
| 15 | Evaluating and debugging neural networks: curves, diagnosis, hyperparameter tuning basics | Analyze, Evaluate |
| 16 | Capstone project presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded notebooks/scripts, Labs 1–15 |
| Assignments (4) | 20% | Problem sets tied to Weeks 4, 8, 11, 14 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Capstone Project | 20% | Proposal (Wk 11) + implementation + presentation (Wk 16) |
| Final Exam | 15% | Comprehensive, emphasis on Weeks 9–16 |

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab
- NumPy (mandatory for Weeks 1–11 from-scratch implementations), Matplotlib
- PyTorch or TensorFlow/Keras (Weeks 12–16)
- Git/GitHub for lab and project submission

## 9. Reference Textbooks

- Goodfellow, I., Bengio, Y., & Courville, A. — *Deep Learning*. MIT Press. (primary reference for
  backpropagation, optimization, and regularization theory).
- Nielsen, M. — *Neural Networks and Deep Learning* (free online book, neuralnetworksanddeeplearning.com).
  Followed closely for the perceptron-to-backpropagation derivation sequence (Weeks 1–8).
- Géron, A. — *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*. O'Reilly.
  (used for the applied framework chapters, Weeks 12–14).
- Official NumPy, PyTorch, and Keras documentation.

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done in
pairs with clearly attributed contributions. Code plagiarism (including uncredited AI-generated
code submitted as original work) is handled per institutional academic integrity policy.
