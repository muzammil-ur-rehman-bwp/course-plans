# Course Plan: Deep Learning (Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Deep Learning |
| Level | Graduate (MS Computer Science / Software Engineering / AI) |
| Credit Hours | 3 (2 hrs lecture + 1 lab session of 3 hrs/week) |
| Prerequisites | **Introduction to Deep Learning (or equivalent)** — students must already be comfortable with deep MLPs, CNN basics (convolution, pooling, the LeNet/AlexNet/VGG/ResNet lineage at a survey level), LSTM/GRU internals, attention as a mechanism, a Transformer **survey** (architecture diagram level, not full derivation), autoencoders, the VAE (reparameterization, ELBO), and GAN basics (minimax game, mode collapse); this course does **not** re-teach any of that. **AND familiarity with basic MDP/RL concepts from an AI course** — states/actions/transitions/rewards, the Bellman equation, value iteration, policy iteration, and tabular Q-learning (as in *Artificial Intelligence*, Graduate, Weeks 8–9) — assumed, not re-derived. Strong Python and working PyTorch fluency required. |
| Programming Language | Python 3.x |
| Core Libraries | PyTorch (primary framework throughout), torchvision, `torch_geometric` (graph neural network weeks), Matplotlib |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + Lab (hands-on, Jupyter/Colab based, GPU access assumed/recommended) |

## 2. Course Description

This is a graduate-depth treatment of **modern deep learning architectures and training
techniques**, picking up exactly where the undergraduate survey course and the foundational
graduate AI course leave off. Where *Introduction to Deep Learning* surveys CNNs, RNNs, attention,
a Transformer architecture diagram, and basic generative models, and where *Artificial
Intelligence* (Graduate) builds the tabular MDP/RL formalism (Bellman equation, value/policy
iteration, tabular Q-learning), this course goes to genuine technical depth on the architectures
and techniques that define the field today: the full Transformer architecture derived and
implemented from scratch (scaled dot-product attention, multi-head attention, positional
encoding, encoder-decoder composition), advanced CNN architectures (ResNet's residual formulation
in depth, DenseNet, efficiency-focused designs), self-supervised and contrastive representation
learning, diffusion models in depth (the forward noising process, the reverse denoising process,
the simplified training objective, sampling), graph neural networks (message passing, GCN,
GraphSAGE, GAT), deep reinforcement learning as the neural-network function-approximation
extension of tabular RL (DQN, policy gradients, actor-critic), large-scale training practices
(mixed precision, gradient checkpointing, parallelism, efficient fine-tuning), and a grounded,
non-hype survey of multimodal/foundation models. The course closes with a research-style capstone.

**This course is deliberately scoped to avoid duplicating three sibling courses.** It does **not**
re-teach basic CNN/RNN/attention concepts (that is *Introduction to Deep Learning*'s territory,
assumed here); it does **not** re-derive the basic MDP formalism, the Bellman equation, or tabular
Q-learning (that is *Artificial Intelligence*, Graduate's territory — Weeks 10–11 here build
directly on top of it); and it does **not** own neural-network theory or training-dynamics
derivations such as backpropagation-as-reverse-mode-AD, initialization theory, normalization
theory, loss-landscape geometry, or generalization theory (that is *Artificial Neural Network*,
Graduate's territory — this course is architecture-and-technique focused, not theory focused, and
cross-references that course's results rather than re-deriving them).

## 3. Goals

- Build directly on the undergraduate CNN/RNN/attention/Transformer-survey foundation and the
  graduate tabular-MDP/RL foundation, without re-teaching either, to study genuinely deeper
  architectures and techniques.
- Derive and implement the full Transformer architecture from scratch: scaled dot-product
  attention, multi-head attention, sinusoidal positional encoding, and encoder-decoder
  composition, and understand encoder-only, decoder-only, and vision-Transformer variants.
- Understand advanced CNN architectures (ResNet, DenseNet, depthwise-separable/efficiency
  designs) at a depth beyond the undergraduate survey.
- Understand self-supervised and contrastive representation learning, including the InfoNCE loss
  and why pretext-task pretraining produces transferable representations.
- Understand modern deep generative modeling beyond VAEs/GANs: normalizing flows and diffusion
  models, including the forward/reverse process equations and the training objective.
- Understand graph neural networks: the message-passing framework, GCN, GraphSAGE, and GAT.
- Extend tabular reinforcement learning to deep reinforcement learning: DQN (experience replay,
  target networks) and policy-gradient/actor-critic methods, including the REINFORCE estimator's
  derivation.
- Acquire practical judgment for large-scale training: data/model parallelism, mixed precision,
  gradient checkpointing, and efficient fine-tuning (LoRA).
- Critically and accurately survey multimodal/foundation models without hype, and situate them as
  natural (if resource-intensive) extensions of the architectures and techniques covered.
- Design, execute, and present an original research-style capstone project: a literature review,
  a reproduced or extended experiment, a written paper, and a conference-style presentation.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the assumed undergraduate DL survey and the assumed tabular-MDP/RL foundation at graduate pace, and map the architecture/technique landscape this course covers. | Remember, Understand |
| CLO2 | Analyze advanced CNN architectures (ResNet's residual formulation, DenseNet's dense connectivity, depthwise-separable efficiency designs) and apply them in PyTorch. | Apply, Analyze |
| CLO3 | Derive scaled dot-product attention, multi-head attention, and sinusoidal positional encoding from first principles, and implement a Transformer encoder block from scratch. | Apply, Analyze |
| CLO4 | Analyze encoder-only (masked-language-modeling), decoder-only (causal/autoregressive), and vision-Transformer variants, and explain their architectural and training differences. | Analyze |
| CLO5 | Derive the InfoNCE contrastive loss and explain why self-supervised pretext-task pretraining produces transferable representations. | Understand, Analyze |
| CLO6 | Derive the normalizing-flow change-of-variables formula and the diffusion model's forward/reverse process equations and simplified training objective, and implement a small instance of each. | Apply, Analyze |
| CLO7 | Derive the Graph Convolutional Network layer equation from the message-passing framework, and apply GraphSAGE- and GAT-style aggregation to graph-structured data. | Apply, Analyze |
| CLO8 | Extend tabular Q-learning to Deep Q-Networks (experience replay, target networks) and derive the REINFORCE policy-gradient estimator, implementing both on toy environments. | Apply, Analyze |
| CLO9 | Evaluate large-scale training practices (data/model parallelism, mixed precision, gradient checkpointing, LoRA) and a grounded survey of multimodal/foundation models, including their compute/data costs and limitations. | Evaluate |
| CLO10 | Critique a deep learning research paper, and design, execute, and present an original research-style capstone: literature review, a reproduced or extended experiment, a written paper, and a conference-style talk. | Analyze, Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Graduate Foundations & Advanced Architectures | 1–4 | Remember, Understand, Apply, Analyze | Course landscape; advanced CNNs; the Transformer derived from scratch; Transformer variants |
| Self-Supervised & Generative Models | 5–7 | Understand, Apply, Analyze | Contrastive learning; normalizing flows; diffusion models |
| Graph Neural Networks & Deep RL | 8–11 | Apply, Analyze | Message passing, GCN/GraphSAGE/GAT; midterm; DQN; policy gradients/actor-critic |
| Scaling, Multimodal Models, Research & Capstone | 12–16 | Evaluate, Create | Large-scale training practices; multimodal/foundation model survey; research methods; capstone |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Graduate DL overview: course goals, rapid review of assumed prerequisites, the architecture-and-technique landscape | Remember, Understand |
| 2 | Advanced CNN architectures: ResNet residual formulation in depth, DenseNet, efficiency-focused designs (depthwise-separable convolutions) | Apply, Analyze |
| 3 | The Transformer architecture from scratch: scaled dot-product attention, multi-head attention, sinusoidal positional encoding, encoder-decoder, layer-norm placement | Apply, Analyze |
| 4 | Transformer variants: encoder-only (BERT-style MLM), decoder-only (GPT-style causal), Vision Transformer (ViT) | Analyze |
| 5 | Self-supervised and contrastive representation learning: pretext tasks, InfoNCE, SimCLR-style framing, linear probing | Understand, Analyze |
| 6 | Advanced generative models I: normalizing flows (change of variables), diffusion models' forward noising process | Understand, Apply |
| 7 | Advanced generative models II: diffusion reverse process, simplified training objective, sampling; contrast with VAE/GAN | Apply, Analyze |
| 8 | Graph Neural Networks I: message passing, GCN layer equation; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Graph Neural Networks II: GraphSAGE, Graph Attention Networks, applications | Remember–Analyze |
| 10 | Deep Reinforcement Learning I: Deep Q-Networks — experience replay, target networks, function-approximation instability | Apply, Analyze |
| 11 | Deep Reinforcement Learning II: policy gradients (REINFORCE, derived), actor-critic methods | Apply, Analyze |
| 12 | Large-scale training practices: data/model parallelism, mixed precision, gradient checkpointing, LoRA | Analyze, Evaluate |
| 13 | Multimodal and foundation models (grounded survey): vision-language models, pretrain-then-adapt, compute/data costs | Evaluate |
| 14 | Research methods: reading/critiquing DL papers, reproducibility challenges in DL; capstone work time | Analyze, Evaluate |
| 15 | Research project work session: literature review and experiment work time, presentation practice with peer feedback | Analyze, Evaluate, Create |
| 16 | Capstone research presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 15% | Graded notebooks, submitted weekly (Labs 1–15) |
| Assignments (3 problem sets) | 15% | Tied to Weeks 4, 7, 11 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 14–15 research-methods assignment; critique of a deep learning paper + in-class presentation |
| Midterm Exam | 15% | Week 9, covers Weeks 1–8 |
| Research Capstone | 25% | Literature review + reproduced/extended experiment + paper + presentation (topic selection Wk 7–8, proposal due Wk 9, work session Wk 15, presentation Wk 16) |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–16 |

**Rationale for the graduate weighting.** As in the sibling graduate courses, weight shifts away
from high-stakes closed-book exams (Midterm + Final total 25%) and toward sustained, research-
style and architecture-implementation work: labs that implement real architectures from scratch
(15%), a dedicated Paper Critique & Presentation component (10%), and a heavy Research Capstone
(25%) requiring a genuine literature review and a reproduced or extended experiment.

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (proposal, draft, final submission) have fixed
deadlines because of the downstream presentation schedule; late capstone milestones are handled
case-by-case with the instructor.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab (GPU access assumed/recommended throughout — a free Colab GPU
  runtime is sufficient for every lab in this course; a local GPU is a convenience, not a
  requirement)
- PyTorch (primary framework for the entire course)
- torchvision (datasets, pretrained models, transforms) for CNN and vision-Transformer weeks
- `torch_geometric` (or an equivalent minimal from-scratch implementation where installation is
  impractical) for the graph neural network weeks
- Matplotlib for visualization throughout
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Textbooks

- Goodfellow, I., Bengio, Y., & Courville, A. — *Deep Learning*. MIT Press. (reference for
  generative model foundations, which this course extends into flows and diffusion).
- Zhang, A., Lipton, Z. C., Li, M., & Smola, A. J. — *Dive into Deep Learning* (free online book,
  d2l.ai). Followed for architecture detail and runnable PyTorch code throughout, especially the
  Transformer, CNN, and attention chapters.
- Vaswani et al., which introduced the Transformer architecture — the primary source for Week 3's
  from-scratch derivation.
- He et al. on deep residual learning (ResNet), used for Week 2's residual-formulation depth.
- Huang et al. on densely connected convolutional networks (DenseNet), used for Week 2.
- Devlin et al. on BERT-style masked language model pretraining, used for Week 4.
- Radford et al. on GPT-style decoder-only causal language modeling, used for Week 4.
- Dosovitskiy et al. on the Vision Transformer (treating image patches as tokens), used for Week 4.
- Chen et al. on SimCLR-style contrastive representation learning, and Oord et al. on the InfoNCE
  loss (contrastive predictive coding), used for Week 5.
- Rezende & Mohamed, and Dinh et al., on normalizing flows, used for Week 6.
- Ho et al. on Denoising Diffusion Probabilistic Models, used for Weeks 6–7.
- Kipf & Welling on Graph Convolutional Networks, Hamilton et al. on GraphSAGE, and Veličković et
  al. on Graph Attention Networks, used for Weeks 8–9.
- Mnih et al. on Deep Q-Networks, used for Week 10.
- Williams on the REINFORCE algorithm, used for Week 11.
- Hu et al. on LoRA (low-rank adaptation), used for Week 12.
- Radford et al. on CLIP-style contrastive vision-language pretraining, used for Week 13.
- Official PyTorch, torchvision, and PyTorch Geometric documentation.

Where a specific year or venue is not given above, students are expected to locate the primary
paper themselves (via the venue/search terms given in lecture) rather than rely on an invented
citation; all results referenced in this course are real, well-established, and correctly
attributed to their actual authors.

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research capstone may be done in
pairs with clearly attributed contributions. Any use of another author's ideas, text, code, or
results — including figures or results from a paper being critiqued or reproduced — must be
properly cited; uncredited reuse of a paper's text or another student's code (including
uncredited AI-generated code or text submitted as original work) is handled per institutional
academic integrity policy. Reproducing a published experiment is expected and encouraged for the
capstone; presenting someone else's reported results as one's own experimental findings is not.
