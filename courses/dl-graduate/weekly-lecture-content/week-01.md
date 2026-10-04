# Week 1 — Lecture Content: Graduate Deep Learning Overview

## 1. Where This Course Sits

This course assumes two prior courses and does not re-teach either:

- **_Introduction to Deep Learning_ (undergraduate):** deep MLPs; CNN basics (convolution,
  pooling, the LeNet → AlexNet → VGG → ResNet survey); LSTM/GRU internals; attention as a
  mechanism; the Transformer at **survey/diagram** level; autoencoders; the VAE (reparameterization,
  ELBO); GAN basics (minimax game, mode collapse); transfer learning.
- **_Artificial Intelligence_, Graduate (Weeks 8–9):** the MDP formalism (states, actions,
  transition model $P(s'\mid s,a)$, reward $R(s,a,s')$, discount $\gamma$); the Bellman equation;
  value iteration; policy iteration; tabular Q-learning with an $\epsilon$-greedy policy.

Two sibling graduate courses exist alongside this one and are **not** duplicated here:

- **_Artificial Neural Network_, Graduate** owns neural-network **theory and training dynamics**:
  automatic differentiation formalized as reverse-mode AD, initialization theory, normalization
  theory, loss-landscape geometry, and generalization theory (PAC/VC/Rademacher, double descent).
  Where this course mentions why a technique works at a mechanism level (e.g., "a residual
  connection eases gradient flow"), the *landscape-geometry proof* of that claim belongs to ANN,
  Graduate, not here.
- **_Machine Learning_, Graduate** owns classical statistical ML theory (SVMs, ensemble methods,
  statistical learning theory) — not referenced further in this course.

This course is **architecture-and-technique focused**: it goes to genuine depth on what the
undergraduate course only surveyed, and extends the AI course's tabular RL to deep RL.

## 2. The Architecture-and-Technique Landscape

| Weeks | Module | What's new beyond the prerequisites |
|---|---|---|
| 2 | Advanced CNNs | ResNet in depth, DenseNet, depthwise-separable efficiency designs |
| 3–4 | Transformers | Full derivation from scratch; BERT/GPT/ViT variants |
| 5 | Self-supervised learning | InfoNCE, contrastive pretraining, linear probing |
| 6–7 | Advanced generative models | Normalizing flows; diffusion models (forward + reverse) |
| 8–9 | Graph neural networks | Message passing, GCN, GraphSAGE, GAT |
| 10–11 | Deep RL | DQN, policy gradients (REINFORCE), actor-critic |
| 12 | Large-scale training | Parallelism, mixed precision, gradient checkpointing, LoRA |
| 13 | Multimodal/foundation models | Grounded survey, CLIP-style contrastive pretraining |
| 14–16 | Research methods & capstone | Paper critique, reproducibility, capstone project |

## 3. A Fluency Check (Not a Re-Derivation)

Students should be able to state the following **without** this course re-deriving them:

- The convolution output-size formula and why parameter sharing reduces CNN parameter counts
  relative to a fully-connected layer on the same input.
- The LSTM/GRU gate equations and why gating mitigates vanishing gradients in recurrent nets.
- Scaled dot-product attention's role as "a learned weighted combination of values," at the
  survey level (Week 3 re-derives this at full rigor, building on — not repeating — this).
- The VAE's ELBO (reconstruction + KL term) and the GAN's minimax objective.
- The Bellman equation $V^*(s) = \max_a \sum_{s'} P(s'\mid s,a)\left[R(s,a,s') + \gamma
  V^*(s')\right]$ and the tabular Q-learning update $Q(s,a) \leftarrow Q(s,a) + \alpha\left[r +
  \gamma \max_{a'} Q(s',a') - Q(s,a)\right]$.

## 4. Environment Setup

```python
import torch, torchvision
print(torch.__version__, torchvision.__version__)
print("CUDA available:", torch.cuda.is_available())

try:
    import torch_geometric
    print("torch_geometric available:", torch_geometric.__version__)
except ImportError:
    print("torch_geometric not installed; Weeks 8-9 provide from-scratch alternatives.")
```

GPU access (a free Colab GPU runtime) is assumed/recommended from Week 2 onward (CNN/Transformer
training) but not strictly required for every lab; Weeks where a toy/synthetic dataset is used
(flows, diffusion on 1-D/2-D data, RL on small toy environments) run comfortably on CPU.

## 5. In-Class Exercise

Sort the following five items into the week of this course's syllabus they belong to, and justify
each placement in one sentence: (a) "a paper proposing a new way to sample from a diffusion
model faster"; (b) "a paper comparing attention-weighted vs. mean neighbor aggregation on
citation graphs"; (c) "a paper fine-tuning a large pretrained model with a rank-8 update"; (d) "a
paper pretraining a joint image-text embedding via a contrastive loss"; (e) "a paper using a
replay buffer and a frozen target network to stabilize training."
