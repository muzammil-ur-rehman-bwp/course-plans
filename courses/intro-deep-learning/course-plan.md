# Course Plan: Introduction to Deep Learning

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Introduction to Deep Learning |
| Level | Undergraduate (3rd/4th year, BS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | **Introduction to Artificial Neural Networks (or equivalent)** — students must already be able to describe the perceptron and the multi-layer perceptron, derive and implement backpropagation, and explain basic gradient-based optimizers (SGD, momentum, RMSProp, Adam) and basic regularization (L1/L2, dropout, early stopping). This course does **not** re-derive backpropagation or re-teach basic perceptron/MLP theory or basic optimizer math; it assumes and builds directly on top of that foundation. |
| Programming Language | Python 3.x |
| Core Libraries | PyTorch (primary framework for the entire course), torchvision, torchtext-equivalent text utilities, Matplotlib |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab (hands-on, Jupyter/Colab based, GPU recommended) |

## 2. Course Description

This course is a full semester on modern deep learning, taken immediately after *Introduction to
Artificial Neural Networks*. Where that prerequisite course builds the perceptron-to-MLP
foundation from first principles — deriving backpropagation by hand, implementing it from scratch
in NumPy, and studying optimizers and regularization in mathematical detail, closing with only a
brief, conceptual two-week mention of CNNs and RNNs — this course assumes all of that is already
in place and goes deep into the architectures and practices that define contemporary deep
learning. Students study convolutional neural networks in depth (convolution arithmetic, the
LeNet-to-ResNet architecture evolution, transfer learning), recurrent architectures in depth (the
LSTM and GRU cell internals, sequence-to-sequence modeling), attention mechanisms and an
architectural survey of the Transformer, generative modeling (autoencoders, variational
autoencoders, and generative adversarial networks), and the optimization, regularization, and
practical considerations that matter when training deep networks at scale. PyTorch is used as the
course's working tool from Week 1 onward — there is no from-scratch NumPy re-implementation phase,
because that phase belongs to the prerequisite course. The course closes with a capstone project in
which students design, train, and critically evaluate a real deep learning model end to end.

## 3. Goals

- Build directly on prerequisite knowledge of the perceptron, MLP, backpropagation, and basic
  optimizers/regularization, without re-deriving it, to study genuinely deeper architectures.
- Understand convolutional neural networks in depth: convolution arithmetic, the architectural
  innovations (depth, skip connections) that made very deep CNNs trainable, and transfer learning.
- Understand recurrent architectures in depth: why vanilla RNNs struggle with long sequences, and
  how the LSTM and GRU gating mechanisms address this, including their exact gate equations.
- Understand attention as a mechanism and the Transformer as a high-level architecture, as the
  bridge from sequence models to the dominant architecture family in modern deep learning.
- Understand generative modeling: autoencoders, variational autoencoders (the reparameterization
  trick and the ELBO), and generative adversarial networks (the minimax game and its failure modes).
- Acquire practical, large-scale training judgment: data augmentation, learning rate schedules,
  mixed-precision training, weight decay vs. L2 regularization under Adam, and debugging networks
  that fail to train.
- Design, train, and critically evaluate an original deep learning project as a capstone,
  including a required evaluation and a written discussion of design trade-offs.
- Reason about the ethical and practical costs (bias, compute/environmental cost) of deep learning
  systems, and situate current trends (foundation models, self-supervised pretraining) accurately.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall and build on prerequisite knowledge (perceptron, MLP, backpropagation, optimizers) to explain why deeper networks require additional machinery (initialization, normalization, skip connections). | Remember, Understand |
| CLO2 | Apply convolution arithmetic and classic-to-modern CNN architectural patterns (LeNet, AlexNet, VGG, ResNet) to build, train, and fine-tune image classifiers in PyTorch. | Apply |
| CLO3 | Analyze the internal gating mechanisms of the LSTM and GRU cell, and apply them to build sequence models for text and time-series tasks. | Analyze, Apply |
| CLO4 | Analyze the attention mechanism's role in overcoming the fixed-context bottleneck of basic sequence-to-sequence models, and describe the Transformer's self-attention, multi-head attention, and positional encoding at an architectural level. | Analyze, Understand |
| CLO5 | Apply generative modeling techniques (autoencoders, VAEs, GANs) to build simple generative models in PyTorch, and evaluate their characteristic training behaviors and failure modes. | Apply, Evaluate |
| CLO6 | Evaluate and apply practical large-scale training techniques (regularization under Adam, learning rate schedules, mixed precision, transfer-learning workflows) and debug networks that fail to train. | Analyze, Evaluate |
| CLO7 | Design, train, and present an original deep learning project on a real dataset, with a justified evaluation and a written discussion of design trade-offs, and critically assess the ethical and resource costs of deep learning systems. | Create, Evaluate |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Framework Transition & Deep Net Practice | 1–2 | Remember, Understand, Apply | PyTorch tour; initialization, batch norm, dropout, gradient flow revisited at depth |
| Convolutional Architectures | 3–5 | Apply, Analyze | Convolution arithmetic, architecture evolution, transfer learning at scale |
| Sequence Models & Attention | 6–9 | Analyze, Understand | LSTM/GRU internals, seq2seq, attention, Transformer survey, midterm |
| Generative Models | 10–12 | Apply, Analyze | Autoencoders, VAEs, GANs |
| Practice, Ethics & Synthesis | 13–16 | Evaluate, Create | Optimization/regularization revisited, practical workflows, ethics, capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | The deep learning landscape; prerequisite recap (stated, not re-taught); PyTorch tour (tensors, autograd, `nn.Module`, training loop) | Remember, Understand |
| 2 | Deep networks in practice: weight initialization (Xavier/He), batch normalization, dropout revisited, vanishing/exploding gradients revisited | Understand, Apply |
| 3 | Convolutional Neural Networks I: convolution arithmetic, pooling, parameter sharing | Understand, Apply |
| 4 | Convolutional Neural Networks II: LeNet → AlexNet → VGG → ResNet; building a CNN on CIFAR-10/Fashion-MNIST | Apply, Analyze |
| 5 | Training deep networks at scale: data augmentation, LR schedules/warmup, transfer learning & fine-tuning a pretrained ResNet | Apply, Analyze |
| 6 | Sequence models I: vanilla RNN limitations; the LSTM cell in depth; the GRU | Understand, Analyze |
| 7 | Sequence models II: sequence-to-sequence architectures, applications, building an LSTM in PyTorch | Apply |
| 8 | Attention mechanisms: the information bottleneck problem, attention scores/weights; midterm review | Analyze |
| 9 | **Midterm Exam** + Introduction to Transformers: self-attention, multi-head attention, positional encoding, encoder-decoder architecture | Remember–Analyze |
| 10 | Autoencoders: reconstruction, dimensionality reduction/denoising, connection to PCA | Understand, Apply |
| 11 | Generative models I: Variational Autoencoders — reparameterization trick, the ELBO | Understand, Apply |
| 12 | Generative models II: Generative Adversarial Networks — minimax game, training dynamics, failure modes | Understand, Analyze |
| 13 | Optimization & regularization revisited: weight decay vs. L2 under Adam, label smoothing, mixed precision, large-batch training | Analyze, Evaluate |
| 14 | Practical deep learning workflows: end-to-end transfer learning, saving/loading/exporting models, debugging networks that won't train | Apply, Evaluate |
| 15 | Ethics and current trends: bias amplification, compute/environmental cost, foundation models and self-supervised pretraining | Evaluate |
| 16 | Capstone project presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 20% | Graded notebooks, Labs 1–15 |
| Assignments (4) | 20% | Problem sets tied to Weeks 5, 7, 9, 14 |
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
- Jupyter Notebook / Google Colab (GPU access assumed/recommended throughout — a free Colab GPU
  runtime is sufficient for every lab in this course; a local GPU is a convenience, not a
  requirement)
- PyTorch (primary framework for the entire course, Weeks 1–16)
- torchvision (datasets, pretrained models, image transforms), and text/sequence utilities as
  needed for sequence-model weeks
- Matplotlib for visualization throughout
- Git/GitHub for lab and project submission

## 9. Reference Textbooks

- Goodfellow, I., Bengio, Y., & Courville, A. — *Deep Learning*. MIT Press. (primary reference for
  CNN, RNN, regularization, and generative model theory; chapters on autoencoders and generative
  models are used directly in Weeks 10–12).
- Zhang, A., Lipton, Z. C., Li, M., & Smola, A. J. — *Dive into Deep Learning* (free online book,
  d2l.ai). The primary applied reference for this course: followed closely for CNN architectures,
  RNN/LSTM/GRU, attention, and the Transformer (Weeks 3–9), with runnable PyTorch code throughout.
- Vaswani, A., et al. — "Attention Is All You Need" (2017). The original paper introducing the
  Transformer architecture; used as the primary source for Week 9's architectural survey.
- Official PyTorch and torchvision documentation.

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The capstone project may be done in
pairs with clearly attributed contributions. Code plagiarism (including uncredited AI-generated
code submitted as original work) is handled per institutional academic integrity policy.
