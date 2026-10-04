# Course Contents: Introduction to Deep Learning

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

This course assumes *Introduction to Artificial Neural Networks* (or equivalent) as a
prerequisite. The perceptron, the multi-layer perceptron, the backpropagation derivation, and
basic optimizer/regularization math are **not** re-taught; they are referenced as prior knowledge
wherever relevant.

---

## Week 1 — The Deep Learning Landscape and the PyTorch Framework Tour
- **Topics:** Why depth matters (hierarchical, learned feature representations vs. hand-engineered
  features); a landscape tour of modern deep learning (vision, language, speech, generative
  systems); a one-pass recap of prerequisite knowledge — perceptron → MLP → backpropagation →
  basic optimizers — stated explicitly as assumed, not re-derived; PyTorch as this course's
  framework from this point forward: tensors, autograd, `nn.Module`, and the canonical training
  loop.
- **Subtopics/Skills:** creating and manipulating tensors; using `requires_grad` and `.backward()`
  to confirm autograd reproduces backpropagation results already understood from the prerequisite
  course; writing an `nn.Module` subclass; writing the `zero_grad → forward → loss → backward →
  step` training loop idiom.
- **Readings:** d2l.ai Ch. 1 (introduction) and the PyTorch "Learn the Basics" tutorial (tensors,
  autograd, `nn.Module`, training loop sections); Goodfellow et al. Ch. 1 (historical trends, as a
  refresher only).
- **Software:** Python 3.10+, PyTorch, torchvision.

## Week 2 — Deep Networks in Practice: Initialization, Normalization, and Gradient Flow
- **Topics:** Weight initialization strategies revisited at implementation depth — Xavier/Glorot
  initialization (variance-preserving for symmetric activations) and He initialization
  (variance-preserving for ReLU); batch normalization (the algorithm, train-mode vs. eval-mode
  statistics, why it stabilizes and accelerates training); dropout revisited as both a
  regularizer and an implicit ensembling method (a trained network behaves like an average over an
  exponential number of sub-networks); vanishing/exploding gradients revisited with concrete
  mitigations (initialization, normalization, gradient clipping, and a preview of skip
  connections).
- **Subtopics/Skills:** applying `torch.nn.init.xavier_uniform_`/`kaiming_normal_`; using
  `nn.BatchNorm1d`/`nn.BatchNorm2d` correctly with `model.train()`/`model.eval()`; using
  `nn.Dropout`; using `torch.nn.utils.clip_grad_norm_`.
- **Readings:** Goodfellow et al. Ch. 8.4 (parameter initialization), Ch. 7.12 (dropout, as
  deepened here); d2l.ai batch normalization chapter.
- **Software:** PyTorch.

## Week 3 — Convolutional Neural Networks I: Convolution Arithmetic and Pooling
- **Topics:** The convolution operation in depth: kernel/filter size, stride, padding, and the
  output-size formula; receptive field growth across stacked convolutional layers; multi-channel
  convolution (input channels, output channels/filters); pooling (max/average) and its effect on
  spatial resolution and translation robustness; parameter sharing revisited in depth — why it is
  the structural reason CNNs generalize better than MLPs on image data, with an exact parameter-
  count comparison.
- **Subtopics/Skills:** computing output spatial size and receptive field size by hand for a given
  kernel/stride/padding configuration; implementing multi-channel convolution and pooling with
  `nn.Conv2d`/`nn.MaxPool2d`; verifying shapes layer by layer.
- **Readings:** Goodfellow et al. Ch. 9.1–9.5 (the convolution operation, motivation, pooling,
  variants); d2l.ai convolutional neural networks chapter (padding and stride; multiple
  input/output channels).
- **Software:** PyTorch, torchvision.

## Week 4 — Convolutional Neural Networks II: Architecture Evolution and a CNN on CIFAR-10
- **Topics:** The architecture evolution from LeNet (the original conv/pool/dense pattern) to
  AlexNet (depth, ReLU, dropout at scale) to VGG (uniform small 3×3 kernels, very deep stacks) to
  ResNet (the degradation problem in very deep networks, and skip/residual connections as the fix
  — why an identity shortcut makes gradients flow and makes learning an incremental correction
  easier than learning a full transformation from scratch).
- **Subtopics/Skills:** implementing a residual block (`nn.Conv2d` + skip connection with
  addition); building and training a small CNN (LeNet-style or with a residual block) on CIFAR-10
  or Fashion-MNIST; tracking training/validation accuracy across epochs.
- **Readings:** d2l.ai modern convolutional neural networks chapter (AlexNet, VGG, ResNet
  sections); He, K., et al. — "Deep Residual Learning for Image Recognition" (2015), summarized via
  d2l.ai.
- **Software:** PyTorch, torchvision (`torchvision.datasets.CIFAR10`).
- **Assignment 1 assigned** (CNN arithmetic and architectures).

## Week 5 — Training Deep Networks at Scale: Augmentation, Schedules, and Transfer Learning
- **Topics:** Data augmentation (random crop, horizontal flip, color jitter) via
  `torchvision.transforms` and why it acts as a regularizer; learning rate schedules (step decay,
  cosine annealing) and warmup, and why warmup helps stabilize early training with large learning
  rates or large batches; transfer learning and fine-tuning: loading a pretrained model from
  `torchvision.models`, freezing/unfreezing layers, and fine-tuning on a new task.
- **Subtopics/Skills:** composing `torchvision.transforms.Compose` augmentation pipelines; using
  `torch.optim.lr_scheduler` (e.g., `CosineAnnealingLR`, `StepLR`); loading
  `torchvision.models.resnet18(weights=...)`, replacing the final classification layer, and
  fine-tuning on a small dataset.
- **Readings:** d2l.ai fine-tuning chapter; PyTorch official "Transfer Learning for Computer
  Vision" tutorial.
- **Software:** PyTorch, torchvision.
- **Assignment 1 due.**

## Week 6 — Sequence Models I: RNN Limitations, the LSTM Cell, and the GRU
- **Topics:** The vanilla RNN recap, framed specifically as "why vanilla RNNs struggle with long
  sequences" (repeated multiplication by the same recurrent weight matrix across many time steps
  causes vanishing or exploding gradients during backpropagation through time); the LSTM cell in
  depth — the forget gate, input gate, output gate, candidate cell state, and cell state update,
  with full gate equations; the GRU as a simplified gated alternative with fewer parameters
  (update gate and reset gate).
- **Subtopics/Skills:** writing out and explaining each LSTM/GRU gate equation; using `nn.LSTM`
  and `nn.GRU`; tracing input/hidden/cell state tensor shapes through a sequence.
- **Readings:** Goodfellow et al. Ch. 10.7, 10.10 (gated RNNs, LSTM); d2l.ai LSTM and GRU chapters.
- **Software:** PyTorch.

## Week 7 — Sequence Models II: Sequence-to-Sequence Architectures and Applications
- **Topics:** The sequence-to-sequence (encoder-decoder) architecture for mapping a variable-length
  input sequence to a variable-length output sequence; representative applications (text
  generation, time-series forecasting); building and training a small LSTM-based model in PyTorch
  for one of these tasks; teacher forcing during training.
- **Subtopics/Skills:** implementing an encoder LSTM and a decoder LSTM; using the encoder's final
  hidden/cell state to initialize the decoder; generating a sequence autoregressively at inference
  time.
- **Readings:** d2l.ai sequence-to-sequence learning chapter.
- **Software:** PyTorch.
- **Assignment 2 assigned** (sequence models).

## Week 8 — Attention Mechanisms; Midterm Review
- **Topics:** The information bottleneck problem in basic seq2seq (a single fixed-length context
  vector must summarize an arbitrarily long input, which degrades for long sequences); attention
  as a learned weighted combination of all encoder hidden states rather than a single fixed
  summary; computing attention scores (dot-product and additive/Bahdanau forms) and converting
  them to weights via softmax; review of Weeks 1–8 for the midterm.
- **Subtopics/Skills:** implementing dot-product attention score computation and the softmax
  weighting step in PyTorch; computing a weighted context vector from encoder states and attention
  weights.
- **Readings:** Bahdanau, D., Cho, K., & Bengio, Y. — "Neural Machine Translation by Jointly
  Learning to Align and Translate" (2014), summarized via d2l.ai's attention chapter.
- **Software:** PyTorch.
- **Assignment 2 due.**

## Week 9 — Midterm Exam; Introduction to Transformers
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: self-attention (every position attends
  to every other position in the same sequence); multi-head attention at a conceptual level
  (running several attention computations in parallel on different learned projections, then
  combining them); positional encoding (injecting order information since self-attention itself is
  order-agnostic); the overall encoder-decoder Transformer architecture at a high level — this
  week is an architectural survey, not a from-scratch full implementation.
- **Subtopics/Skills:** computing scaled dot-product attention scores for a small example;
  explaining, at a diagram level, how an encoder stack and a decoder stack are composed from
  self-attention, multi-head attention, and feed-forward sublayers.
- **Readings:** Vaswani, A., et al. — "Attention Is All You Need" (2017), the paper that introduced
  the Transformer architecture — primary source for this week; d2l.ai Transformer chapter.
- **Software:** PyTorch.
- **Assignment 3 assigned** (attention and Transformers).

## Week 10 — Autoencoders
- **Topics:** The encoder-decoder architecture applied to unsupervised reconstruction; using the
  bottleneck (latent) layer for dimensionality reduction; denoising autoencoders (reconstructing a
  clean input from a corrupted version); the connection between a linear autoencoder and Principal
  Component Analysis (a linear autoencoder with a squared-error loss learns a subspace spanned by
  the same directions as PCA's top principal components).
- **Subtopics/Skills:** building a small fully-connected or convolutional autoencoder in PyTorch;
  training a denoising variant by adding noise to inputs while reconstructing the clean target;
  visualizing reconstructions and the latent space.
- **Readings:** Goodfellow et al. Ch. 14.1–14.5 (autoencoders, including the PCA connection).
- **Software:** PyTorch.

## Week 11 — Generative Models I: Variational Autoencoders
- **Topics:** The generative modeling problem (learning to sample new, realistic data rather than
  only reconstructing given inputs); the Variational Autoencoder (VAE): a probabilistic encoder
  producing a latent distribution instead of a point, the reparameterization trick for
  differentiable sampling, and the Evidence Lower Bound (ELBO) loss, consisting of a
  reconstruction term and a KL-divergence regularization term.
- **Subtopics/Skills:** implementing the reparameterization trick (`z = mu + sigma * epsilon`);
  implementing the ELBO loss (reconstruction loss + closed-form Gaussian KL term); building and
  training a simple VAE on MNIST/Fashion-MNIST; sampling new images from the trained decoder.
- **Readings:** Kingma, D. P., & Welling, M. — "Auto-Encoding Variational Bayes" (2013), the
  original VAE paper, summarized via d2l.ai/standard course treatment.
- **Software:** PyTorch.
- **Capstone project proposal due.**

## Week 12 — Generative Models II: Generative Adversarial Networks
- **Topics:** The Generative Adversarial Network (GAN): a generator network that maps noise to
  data-like samples, and a discriminator network trained to distinguish real from generated
  samples, framed as a two-player minimax game; the adversarial training loop (alternating
  discriminator and generator updates); common failure modes — mode collapse (the generator
  produces limited variety) and training instability (oscillating or diverging losses), and why
  GAN training is harder to diagnose than supervised training.
- **Subtopics/Skills:** implementing a simple generator and discriminator (`nn.Sequential` MLPs or
  small CNNs); writing the alternating adversarial training loop; recognizing mode collapse from
  generated-sample diversity and loss curve behavior.
- **Readings:** Goodfellow, I., et al. — "Generative Adversarial Networks" (2014), the original GAN
  paper, summarized via standard course treatment.
- **Software:** PyTorch.

## Week 13 — Optimization and Regularization for Deep Nets, Revisited
- **Topics:** Weight decay vs. L2 regularization — mathematically identical under plain SGD, but
  subtly different under Adam (Adam's adaptive per-parameter scaling interacts with an L2 penalty
  added to the gradient, which motivated AdamW's decoupled weight decay); label smoothing as a
  regularizer for classification (softening one-hot targets to discourage overconfidence);
  mixed-precision training at a conceptual level (using float16/bfloat16 compute with a float32
  master copy and loss scaling to preserve numerical stability while speeding up training); large-
  batch training considerations (linear learning-rate scaling with batch size, and why warmup
  from Week 5 matters more at large batch sizes).
- **Subtopics/Skills:** using `torch.optim.AdamW` and explaining its difference from
  `Adam` with an L2 penalty; implementing label smoothing (via `nn.CrossEntropyLoss(label_smoothing=...)`
  or manually); describing, at a conceptual level, a `torch.cuda.amp.autocast`/`GradScaler`
  mixed-precision training loop.
- **Readings:** Loshchilov, I., & Hutter, F. — "Decoupled Weight Decay Regularization" (2017,
  the AdamW paper), summarized; Goodfellow et al. Ch. 8 (revisited, optimization in practice).
- **Software:** PyTorch.
- **Assignment 4 assigned** (generative models and practical training).

## Week 14 — Practical Deep Learning Workflows
- **Topics:** A full transfer-learning project workflow end to end (data loading → augmentation →
  pretrained backbone → fine-tuning → evaluation → iteration); saving and loading models
  (`state_dict`, checkpoints) and exporting models for deployment, mentioned conceptually
  (TorchScript tracing/scripting, ONNX export); debugging a deep network that will not train
  (checking data/label correctness, verifying a single batch can be overfit, checking for
  forgotten `model.train()`/`model.eval()` switches, inspecting gradient norms, and
  learning-rate sanity checks).
- **Subtopics/Skills:** assembling an end-to-end training script from components built in prior
  weeks; using `torch.save`/`torch.load` with `state_dict`; working through a provided "broken"
  training script and diagnosing the fault.
- **Readings:** Goodfellow et al. Ch. 11 (practical methodology, revisited); PyTorch "Saving and
  Loading Models" tutorial.
- **Software:** PyTorch, torchvision.

## Week 15 — Ethics and Current Trends
- **Topics:** Bias amplification in deep models (how skewed training data and proxy features can
  cause a model to amplify, not just reflect, existing disparities); the environmental and compute
  cost of large-scale model training (FLOPs, energy use, and the trend of growing training cost
  relative to prior architectures); a grounded, non-hype survey of current trends — foundation
  models (large pretrained models adapted to many downstream tasks) and self-supervised
  pretraining (learning useful representations from unlabeled data) — presented as natural
  extensions of transfer learning (Week 5) and the Transformer (Week 9), not as unexplained hype.
- **Subtopics/Skills:** critically reading a case study of biased model behavior and identifying
  the likely data/design cause; estimating relative training compute cost from parameter count and
  training steps for two example models.
- **Readings:** selected sections of Goodfellow et al. on generalization and fairness-adjacent
  discussion where available; current, well-established overview material on foundation models and
  self-supervised pretraining (lecture notes synthesize rather than quote a single source).
- **Software:** none required beyond a text editor/notebook for the case-study discussion.

## Week 16 — Capstone Presentations, Course Review
- **Topics:** Student capstone project presentations; recap of the course map (deep networks in
  practice → CNNs → sequence models → attention/Transformers → generative models → optimization
  and practice at scale → ethics); discussion of where these foundations lead next (advanced
  architecture, graduate-level deep learning, and specialized follow-on courses).
- **Deliverable:** Capstone project final submission + presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Capstone Project (introduced Week 9 discussion, proposal Week 11, final Week 16)
Students (individually or in pairs) design, train, and evaluate a deep learning project end to end
using PyTorch — for example, a CNN image classifier built with transfer learning from a pretrained
`torchvision.models` backbone, an LSTM-based text-generation or time-series forecasting model, or
a simple VAE or GAN on an image dataset — and must include a required evaluation appropriate to
the chosen task (e.g., test accuracy and a confusion matrix for classification; reconstruction
loss and qualitative samples for a VAE; discriminator/generator loss curves and sample quality for
a GAN) along with a written discussion of the design choices and trade-offs made (architecture
choice, hyperparameters, and at least one thing that did not work as expected and why), reported
honestly in a short written report and a 5–7 minute presentation.
