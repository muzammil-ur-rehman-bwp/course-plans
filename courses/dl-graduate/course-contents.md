# Course Contents: Deep Learning (Graduate)

Detailed per-week breakdown of topics, subtopics, and resources. Companion to `course-plan.md`.
Each week lists: **Topics**, **Subtopics/Skills**, **Readings**, **Software/Libraries used**.

This course assumes *Introduction to Deep Learning* (or equivalent) — deep MLPs, CNN basics,
LSTM/GRU, attention, a Transformer **survey**, autoencoders, VAE, GAN basics, transfer learning —
and familiarity with the tabular MDP/RL formalism from *Artificial Intelligence*, Graduate
(Weeks 8–9: the Bellman equation, value iteration, policy iteration, tabular Q-learning). None of
that foundation is re-taught or re-derived here; it is referenced as prior knowledge throughout.

---

## Week 1 — Graduate Deep Learning Overview
- **Topics:** Course goals and how this course differs from *Introduction to Deep Learning*
  (survey → depth) and from the sibling graduate courses (*Artificial Neural Network*, Graduate
  owns theory/training-dynamics; *Artificial Intelligence*, Graduate owns the tabular MDP/RL
  formalism this course's deep-RL weeks build on); a rapid, explicitly-not-re-taught review of
  assumed prerequisites — deep MLPs, CNN basics (convolution/pooling, the LeNet-to-ResNet survey),
  LSTM/GRU internals, attention as a mechanism, the Transformer at survey/diagram level, VAE/GAN
  basics; a map of the architecture-and-technique landscape this course covers (advanced CNNs,
  the Transformer from scratch, self-supervised/contrastive learning, diffusion models, graph
  neural networks, deep RL, large-scale training, multimodal models).
- **Subtopics/Skills:** stating precisely which prerequisite topics are assumed vs. which this
  course adds depth to; setting up the PyTorch/torchvision/`torch_geometric` environment used
  throughout the semester; identifying, for a given paper title, which later week's topic it
  belongs to.
- **Readings:** d2l.ai, skim of chapters already covered at the undergraduate level (as a
  refresher only); course syllabus.
- **Software:** Python 3.10+, PyTorch, torchvision, Colab GPU runtime check.

## Week 2 — Advanced CNN Architectures
- **Topics:** The ResNet residual-connection formulation in depth — the identity-mapping argument
  for why a residual block eases gradient flow through very deep stacks (a brief cross-reference
  to *Artificial Neural Network*, Graduate's loss-landscape/optimization angle on skip
  connections, not a repeat of that theory); DenseNet's dense connectivity (each layer receives
  the concatenation of all preceding layers' feature maps within a dense block, encouraging
  feature reuse and strengthening gradient flow); efficiency-focused architectures — depthwise
  separable convolutions (factoring a standard convolution into a per-channel depthwise
  convolution followed by a 1×1 pointwise convolution), as used in MobileNet/EfficientNet-style
  designs, and the resulting parameter/FLOP savings.
- **Subtopics/Skills:** implementing a ResNet-style residual block and a DenseNet-style dense
  block in PyTorch; implementing a depthwise separable convolution block
  (`nn.Conv2d(groups=in_channels)` followed by a 1×1 `nn.Conv2d`) and computing its parameter
  count against a standard convolution of the same input/output channel configuration.
- **Readings:** He et al. on deep residual learning (ResNet); Huang et al. on densely connected
  convolutional networks (DenseNet); d2l.ai modern CNN architectures chapter (as a bridge from
  the undergraduate survey).
- **Software:** PyTorch, torchvision.

## Week 3 — The Transformer Architecture From Scratch
- **Topics:** Scaled dot-product attention derived in full — queries, keys, and values; the score
  matrix $QK^\top$; the $1/\sqrt{d_k}$ scaling and why it is needed to prevent softmax saturation
  as $d_k$ grows; multi-head attention (projecting $Q,K,V$ into $h$ learned subspaces, running
  attention in parallel, concatenating and projecting back); sinusoidal positional encoding (the
  exact formula, and why its structure lets the model recover relative position via a linear
  transformation); the full encoder-decoder architecture (stacked self-attention and feed-forward
  sublayers, residual connections around each sublayer, encoder-decoder cross-attention); layer
  normalization placement conventions (the original post-LN placement versus the now-common
  pre-LN placement, and pre-LN's training-stability advantage at depth).
- **Subtopics/Skills:** deriving and implementing scaled dot-product attention and multi-head
  attention from scratch in PyTorch (no `nn.MultiheadAttention` shortcut); implementing sinusoidal
  positional encoding and verifying its relative-position property numerically; implementing one
  Transformer encoder block (multi-head self-attention + feed-forward, residual connections,
  layer norm) end to end.
- **Readings:** Vaswani et al., which introduced the Transformer architecture — primary source for
  this week; d2l.ai Transformer chapter, read at implementation depth (beyond the undergraduate
  survey pass).
- **Software:** PyTorch.
- **Assignment 1 assigned** (advanced CNNs and the Transformer from scratch, Weeks 2–3).

## Week 4 — Transformer Variants and Applications
- **Topics:** Encoder-only pretraining — masked language modeling (BERT-style): randomly masking a
  fraction of input tokens and training the encoder to predict them from bidirectional context;
  decoder-only causal/autoregressive models (GPT-style) — the causal attention mask that prevents
  a position from attending to future positions, and next-token-prediction pretraining; a brief
  look at Vision Transformers (ViT) — treating fixed-size image patches as a token sequence,
  linearly projecting each patch, prepending a learnable class token, and adding learned (or
  sinusoidal) positional embeddings before a standard Transformer encoder.
- **Subtopics/Skills:** implementing a causal attention mask and verifying it blocks
  future-position attention; implementing image-patch extraction and linear projection for a toy
  ViT input pipeline; contrasting encoder-only, decoder-only, and encoder-decoder attention
  masking patterns on paper.
- **Readings:** Devlin et al. on BERT-style masked language model pretraining; Radford et al. on
  GPT-style decoder-only causal language modeling; Dosovitskiy et al. on the Vision Transformer.
- **Software:** PyTorch, torchvision.

## Week 5 — Self-Supervised and Contrastive Representation Learning
- **Topics:** The pretext-task idea (training on a task derived automatically from unlabeled data
  so that solving it requires learning generally useful representations); contrastive learning —
  the InfoNCE loss, derived from treating representation learning as classifying a positive pair
  against a batch of negatives, and a SimCLR-style framing (two randomly augmented views of the
  same image form a positive pair; all other images in the batch supply negatives); why
  self-supervised pretraining on large unlabeled data followed by a small amount of labeled
  fine-tuning (the transfer-learning pattern from the undergraduate course, now applied to
  representations learned without labels) produces useful, transferable representations;
  evaluating representation quality via linear probing (freezing the pretrained encoder and
  training only a linear classifier on top), conceptually.
- **Subtopics/Skills:** deriving the InfoNCE loss from the positive-vs-negatives classification
  framing; implementing an InfoNCE loss and a minimal SimCLR-style training step (two augmented
  views, a shared encoder, a projection head) in PyTorch; describing the linear-probing evaluation
  protocol and why it isolates representation quality from classifier capacity.
- **Readings:** Oord et al. on the InfoNCE loss (contrastive predictive coding); Chen et al. on
  SimCLR-style contrastive visual representation learning.
- **Software:** PyTorch, torchvision.

## Week 6 — Advanced Generative Models I: Normalizing Flows and the Diffusion Forward Process
- **Topics:** Normalizing flows — the change-of-variables formula for probability densities under
  an invertible transformation, and a simple 1-D worked example (e.g., an affine flow) showing how
  a simple base density is reshaped into a more complex one while density evaluation stays exact
  and tractable; the score-based/diffusion modeling idea introduced via its forward process — a
  Markov chain that gradually adds Gaussian noise to data over $T$ steps until the data distribution
  is driven to an (approximately) standard Gaussian, with the closed-form marginal that lets any
  step $x_t$ be sampled directly from $x_0$ without simulating every intermediate step.
- **Subtopics/Skills:** deriving and verifying the 1-D change-of-variables formula for an affine
  normalizing flow; implementing a minimal normalizing flow on 1-D synthetic data in PyTorch and
  visualizing how it reshapes a Gaussian base density; implementing the diffusion forward process
  (adding noise at an arbitrary step $t$ via its closed-form marginal) and visualizing the
  progressive loss of structure as $t$ increases.
- **Readings:** Rezende & Mohamed, and Dinh et al., on normalizing flows; Ho et al. on Denoising
  Diffusion Probabilistic Models (forward process sections).
- **Software:** PyTorch.

## Week 7 — Advanced Generative Models II: The Diffusion Reverse Process and Training
- **Topics:** The diffusion model's reverse denoising process — a learned Markov chain that
  undoes the forward noising process one step at a time, parameterized as a Gaussian with a
  learned mean (typically expressed via a learned noise predictor); the simplified training
  objective (predicting the noise added at a randomly sampled step, the DDPM-style loss) and why
  this simplified objective is tractable where the full variational bound is not; the sampling
  procedure (iteratively applying the learned reverse step from pure noise down to a generated
  sample); contrasting the diffusion approach with the GAN (adversarial minimax training) and VAE
  (single-step encode/decode with an explicit ELBO) approaches already covered at the
  undergraduate level — in particular, diffusion's iterative, many-step generation versus a GAN's
  or VAE's single forward pass, and the resulting sample-quality/training-stability/sampling-speed
  trade-offs.
- **Subtopics/Skills:** implementing the simplified DDPM training loss (sampling a timestep and
  noise, forming the noisy input, and regressing a small network onto the added noise) and the
  corresponding sampling loop, on a toy 1-D or 2-D synthetic dataset; writing a short comparison of
  diffusion vs. VAE vs. GAN sampling and training behavior.
- **Readings:** Ho et al. on Denoising Diffusion Probabilistic Models (reverse process and
  training objective sections).
- **Software:** PyTorch.

## Week 8 — Graph Neural Networks I; Midterm Review
- **Topics:** Representing graphs for learning (adjacency structure, node features, edge
  features); the message-passing framework — at each layer, every node aggregates "messages" from
  its neighbors and updates its own representation (the aggregate-and-update pattern that
  generalizes across almost all GNN variants); the Graph Convolutional Network (GCN) layer
  equation, derived from spectral graph convolution motivation and given in its practical
  spatial/normalized-adjacency form; review of Weeks 1–8 for the midterm.
- **Subtopics/Skills:** implementing a GCN layer from scratch in PyTorch (building the normalized
  adjacency matrix and the layer's matrix form) and stacking two GCN layers for node
  classification on a small graph; tracing one message-passing round by hand on a toy graph.
- **Readings:** Kipf & Welling on Graph Convolutional Networks; d2l.ai or an equivalent GNN
  introduction for the message-passing framing.
- **Software:** PyTorch, `torch_geometric` (or a from-scratch implementation).

## Week 9 — Midterm Exam; Graph Neural Networks II
- **Topics:** Midterm Exam (covers Weeks 1–8). Afterward: GraphSAGE-style neighborhood
  sampling/aggregation (conceptual) — sampling a fixed-size neighborhood per node for scalability
  on large graphs, and aggregating via mean/pooling/LSTM-style aggregators followed by
  concatenation with the node's own prior representation; Graph Attention Networks (GAT) —
  learning attention weights over a node's neighbors rather than using fixed (e.g.,
  degree-normalized) weights, so that a node can weigh different neighbors' messages differently;
  real applications of GNNs (molecule property prediction, treating atoms as nodes and bonds as
  edges; recommendation systems, treating users/items as a bipartite graph), discussed
  conceptually.
- **Subtopics/Skills:** implementing a GraphSAGE-style mean-aggregator layer and a GAT-style
  attention-weighted aggregation layer in PyTorch; comparing GCN, GraphSAGE, and GAT aggregation
  rules on the same small graph and discussing when attention-weighted aggregation should help.
- **Readings:** Hamilton et al. on GraphSAGE; Veličković et al. on Graph Attention Networks.
- **Software:** PyTorch, `torch_geometric` (or a from-scratch implementation).
- **Assignment 2 assigned** (self-supervised/generative models and GNNs, Weeks 5–9).

## Week 10 — Deep Reinforcement Learning I: Deep Q-Networks
- **Topics:** Using a neural network to approximate $Q(s,a)$ for large or continuous state spaces
  where a tabular $Q$-table (assumed from *Artificial Intelligence*, Graduate) is infeasible — this
  week explicitly does **not** re-derive the Bellman equation or the tabular Q-learning update,
  only extends them to function approximation; the Deep Q-Network (DQN) architecture and loss,
  regressing $Q_\theta(s,a)$ toward a target built from the Bellman optimality equation; experience
  replay (storing and uniformly resampling past transitions to break harmful temporal correlation
  and reuse data); target networks (a periodically-updated, frozen copy of the network used to
  compute targets, stabilizing a moving-target regression problem); why naive function
  approximation combined with the plain Q-learning update can diverge, and how experience replay
  and target networks address this in practice.
- **Subtopics/Skills:** implementing a small DQN (an MLP $Q_\theta$), an experience replay buffer,
  and a target network update rule in PyTorch; training DQN on a toy environment (e.g., a small
  custom grid-world or CartPole) and empirically comparing stability with and without a target
  network/replay buffer.
- **Readings:** Mnih et al. on Deep Q-Networks.
- **Software:** PyTorch; `gymnasium` (or an equivalent small toy environment) if available,
  otherwise a custom grid-world environment.

## Week 11 — Deep Reinforcement Learning II: Policy Gradient and Actor-Critic Methods
- **Topics:** Policy gradient methods — directly parameterizing and optimizing a stochastic policy
  $\pi_\theta(a\mid s)$ rather than a value function; the REINFORCE algorithm and its gradient
  estimator, derived via the log-derivative ("score function") trick from the policy-gradient
  theorem; the high-variance nature of the raw REINFORCE estimator and the use of a return
  baseline to reduce it without introducing bias; actor-critic methods (conceptual) — combining a
  learned value-function baseline (the critic) with the policy gradient (the actor) so that the
  advantage $A(s,a) = G_t - V(s)$ replaces the raw return, substantially reducing variance.
- **Subtopics/Skills:** deriving the REINFORCE gradient estimator step by step from the
  policy-gradient theorem; implementing REINFORCE (with a simple return baseline) on a toy
  environment in PyTorch; describing, conceptually, how an actor-critic architecture's critic
  loss and actor loss are computed and updated.
- **Readings:** Williams on the REINFORCE algorithm; Sutton & Barto's policy-gradient chapter
  (already a reference text in *Artificial Intelligence*, Graduate) for the actor-critic framing.
- **Software:** PyTorch; `gymnasium` (or an equivalent small toy environment) if available,
  otherwise a custom grid-world/bandit environment.
- **Assignment 3 assigned** (graph neural networks and deep reinforcement learning, Weeks 8–11).

## Week 12 — Large-Scale Training Practices
- **Topics:** Data parallelism versus model parallelism (conceptual) — splitting a batch across
  devices and synchronizing gradients, versus splitting a single model's layers/parameters across
  devices when it does not fit on one; mixed-precision training in depth — computing in FP16/BF16
  for speed and memory while keeping a master set of weights (or relying on BF16's wider dynamic
  range) for numerical stability, and loss scaling (multiplying the loss before the backward pass
  to keep small FP16 gradients from underflowing, then unscaling before the optimizer step);
  gradient checkpointing (discarding intermediate activations during the forward pass and
  recomputing them during the backward pass, trading extra compute for reduced memory); efficient
  fine-tuning ideas — low-rank adaptation (LoRA), conceptually: freezing a large pretrained
  weight matrix and learning only a low-rank update to it, drastically reducing the number of
  trainable parameters for downstream adaptation.
- **Subtopics/Skills:** describing, precisely, the mixed-precision training loop
  (`torch.autocast` + `torch.cuda.amp.GradScaler`) and what loss scaling corrects for; implementing
  a toy gradient-checkpointing example with `torch.utils.checkpoint` and verifying it trades
  memory for recomputation; implementing a minimal LoRA-style low-rank update
  ($W' = W + BA$, with $B \in \mathbb{R}^{d\times r}$, $A \in \mathbb{R}^{r\times k}$, $r \ll
  \min(d,k)$) wrapped around a frozen linear layer.
- **Readings:** Micikevicius et al. on mixed-precision training; Hu et al. on LoRA (low-rank
  adaptation).
- **Software:** PyTorch (`torch.autocast`, `torch.cuda.amp.GradScaler`, `torch.utils.checkpoint`).

## Week 13 — Multimodal and Foundation Models (Grounded Survey)
- **Topics:** A grounded, non-hype survey of vision-language models — conceptually, learning a
  joint embedding space for images and text via contrastive image-text pretraining (a CLIP-style
  approach: paired image/text encoders trained so that matching image-text pairs have high
  similarity and mismatched pairs have low similarity, directly extending Week 5's contrastive
  framing across two modalities); the pretrain-then-finetune/adapt paradigm at scale (large
  pretrained "foundation" models adapted to downstream tasks via fine-tuning or the parameter-
  efficient methods from Week 12); an honest discussion of the compute and data requirements
  behind large-scale pretraining and the practical limitations of current multimodal/foundation
  models (data quality and bias, evaluation difficulty, compute cost concentrated among a few
  organizations).
- **Subtopics/Skills:** describing the CLIP-style contrastive image-text pretraining objective as
  an extension of Week 5's InfoNCE framing to paired modalities; estimating, from parameter count
  and token/sample count, the relative pretraining compute of two example model configurations;
  critically discussing one concrete limitation of a current multimodal model from a
  course-provided case description.
- **Readings:** Radford et al. on CLIP-style contrastive vision-language pretraining; current,
  well-established overview material on foundation models (lecture notes synthesize rather than
  quote a single source, as this is a fast-moving area).
- **Software:** none required beyond a notebook for the compute-estimation exercise.

## Week 14 — Research Methods in Deep Learning; Capstone Work Time
- **Topics:** How to read and critique a deep learning paper efficiently (abstract → figures/
  results → method → related work → full read; the same structured-critique skill as the
  sibling graduate courses, applied here to DL-specific claims); reproducibility challenges
  specific to deep learning — compute cost as a barrier to reproduction, hyperparameter
  sensitivity (a result that only appears under a narrow, possibly under-disclosed hyperparameter
  setting), and benchmark-culture critiques (leaderboard-chasing, benchmark saturation/
  contamination, and the gap between benchmark performance and real-world robustness); structured
  in-class capstone work time: finalizing topic choice and literature search.
- **Subtopics/Skills:** critiquing a short deep learning paper excerpt as a structured in-class
  exercise, identifying claim, evidence, hyperparameter-sensitivity risk, and a benchmark-culture
  concern if applicable; beginning the capstone literature search (3–5 candidate papers).
- **Readings:** instructor-provided guidance handouts on reading/critiquing DL papers; students
  begin selecting a paper from a suggested list (drawn from NeurIPS/ICML/CVPR-style venues) for
  the Paper Critique & Presentation assignment.
- **Software:** none (methods/discussion week).
- **Paper Critique & Presentation assignment assigned.**

## Week 15 — Research Project Work Session
- **Topics:** Structured, instructor-guided work time for the research capstone — finalizing the
  literature review (3–5 papers), designing and running the small reproduced/extended experiment,
  and drafting the written paper; paper-presentation practice — each student/pair gives a short
  practice run of their capstone talk and receives structured peer feedback before Week 16; the
  Paper Critique & Presentation assignment's in-class presentations also take place this week.
- **Subtopics/Skills:** giving and receiving structured, specific feedback on a research talk
  (clarity of problem statement, soundness of experiment, honesty about limitations); revising a
  draft literature review or experiment plan in response to feedback.
- **Readings:** none assigned; working session on students' own capstone materials.
- **Software:** whatever each capstone project requires (see individual proposals).
- **Deliverable:** capstone written-paper draft due; peer-feedback worksheet submitted; Paper
  Critique & Presentation due.

## Week 16 — Capstone Research Presentations; Course Review
- **Topics:** Student capstone research presentations (conference-talk format: problem, related
  work, method/experiment, results, limitations, Q&A); recap of the course map (advanced CNNs →
  the Transformer from scratch → Transformer variants → self-supervised learning → diffusion
  models → graph neural networks → deep RL → large-scale training → multimodal models → research
  methods → capstone); closing discussion connecting this course's architectural depth back to the
  sibling graduate courses (ANN theory, AI/tabular RL, ML statistical theory) students may draw on
  for further research.
- **Deliverable:** Capstone final paper submission + conference-style presentation.

## Week 17 — Final Exam Week
- Comprehensive final exam, weighted toward Weeks 9–16 content (per Assessment Plan).

---

## Research Capstone (topic selection Weeks 7–8, proposal due Week 9, work session Week 15, presentations Week 16)
Students (individually or in pairs) choose a deep-learning subtopic covered in this course (or
closely adjacent, with instructor approval) and complete a research-style project with four
required components: (1) a **literature review** of 3–5 relevant papers summarizing the state of
the art and the specific gap or question the project addresses; (2) a **small reproduced or
extended experiment** — either reproducing a core result from one of the reviewed papers at small
scale, or extending/varying it in a focused way (e.g., comparing GCN vs. GAT aggregation on the
same small graph, an ablation on diffusion noise-schedule choice, a DQN-vs-REINFORCE comparison on
the same toy environment, or a small LoRA-rank ablation on a fine-tuning task); (3) a **short
written paper** (introduction, related work, method, results, discussion of limitations, in a
conference-short-paper style); and (4) a **conference-style presentation** in Week 16. The
capstone is explicitly research-shaped, not a plain coding project: a project that honestly
reports a negative or partial result, correctly analyzed, is graded on the soundness of its
literature review, experimental design, and analysis — not on whether the original paper's result
was fully reproduced. Example topics: a controlled comparison of ResNet vs. DenseNet parameter
efficiency at matched depth; a small-scale reproduction of SimCLR-style linear-probe accuracy
versus pretraining epoch count; a normalizing-flow vs. diffusion-model comparison on 2-D synthetic
data; an empirical study of GCN vs. GraphSAGE vs. GAT on a small node-classification benchmark; a
DQN ablation isolating the effect of experience replay buffer size; a REINFORCE-with-baseline vs.
plain-REINFORCE variance comparison on a toy environment; a small LoRA-rank-vs-downstream-accuracy
sweep on a fine-tuning task.
