# Week 5 — Lecture Content: Self-Supervised and Contrastive Representation Learning

## 1. The Pretext-Task Idea

A **pretext task** is a task whose labels are derived automatically from unlabeled data itself
(e.g., "predict which of two augmented crops came from the same image," "predict a masked word"),
rather than from human annotation. If solving the pretext task well *requires* the model to learn
features that capture meaningful structure in the data (shape, texture, semantic content), those
features transfer to downstream tasks the model was never explicitly trained on — exactly the
transfer-learning pattern from the undergraduate course, except the pretraining phase now needs
no labels at all, which is why self-supervised pretraining scales to vastly larger, unlabeled
datasets.

## 2. Contrastive Learning and the InfoNCE Loss, Derived

Contrastive learning poses representation learning as a **discrimination task**: given an anchor
representation $z_i$, correctly identify its positive match $z_i^+$ (e.g., a different augmented
view of the same underlying image) out of a set containing the positive plus $K$ negatives
$\{z_k^-\}_{k=1}^K$ (other items in the batch).

Treat this exactly as a $(K{+}1)$-way classification problem, where the "logit" for candidate $z$
is a similarity score $\text{sim}(z_i, z)/\tau$ (cosine similarity is standard; $\tau$ is a
temperature hyperparameter controlling how sharply the distribution concentrates). The
softmax-cross-entropy loss for correctly picking the positive out of the positive-plus-negatives
set is the **InfoNCE loss**:

$$
\mathcal{L}_{\text{InfoNCE}} = -\log \frac{\exp(\text{sim}(z_i, z_i^+)/\tau)}{\exp(\text{sim}(z_i,
z_i^+)/\tau) + \sum_{k=1}^{K} \exp(\text{sim}(z_i, z_k^-)/\tau)}.
$$

This is literally the categorical cross-entropy loss for a $(K{+}1)$-class classification problem
whose "correct class" is the positive — minimizing it directly maximizes similarity to the true
positive relative to all negatives, pulling positive pairs together and pushing negative pairs
apart in representation space. Lower temperature $\tau$ sharpens the distribction (penalizes
near-miss negatives more harshly); higher $\tau$ softens it.

```python
import torch
import torch.nn.functional as F

def info_nce_loss(z1, z2, temperature=0.5):
    """
    z1, z2: (B, D) L2-normalized embeddings of two augmented views of the same B images;
    z1[i] and z2[i] form a positive pair; all other combinations in the batch are negatives.
    """
    z1 = F.normalize(z1, dim=1)
    z2 = F.normalize(z2, dim=1)
    B = z1.size(0)
    representations = torch.cat([z1, z2], dim=0)                 # (2B, D)
    sim = representations @ representations.T / temperature      # (2B, 2B)
    sim.fill_diagonal_(float("-inf"))                             # exclude self-similarity

    # positive of row i (i<B) is row i+B, and vice versa
    targets = torch.cat([torch.arange(B, 2 * B), torch.arange(0, B)]).to(z1.device)
    return F.cross_entropy(sim, targets)
```

## 3. A SimCLR-Style Framing

1. Sample a minibatch of images; for each image, produce **two** independently augmented views
   (random crop, color jitter, flip, etc.) — these form a positive pair.
2. Pass both views through a **shared encoder** $f_\theta$ (e.g., a CNN backbone) to get
   representations, then through a small **projection head** $g_\theta$ (an MLP) to get the
   embeddings $z$ used in the InfoNCE loss.
3. All other images' views in the batch serve as negatives — no labels are used anywhere.
4. After pretraining, the projection head is typically discarded, and the encoder $f_\theta$'s
   representations are used for downstream tasks.

```python
import torch.nn as nn

class SimCLRModel(nn.Module):
    def __init__(self, encoder, encoder_dim, proj_dim=128):
        super().__init__()
        self.encoder = encoder
        self.projector = nn.Sequential(
            nn.Linear(encoder_dim, encoder_dim), nn.ReLU(), nn.Linear(encoder_dim, proj_dim)
        )

    def forward(self, x):
        h = self.encoder(x)
        return self.projector(h)

# one training step
# z1, z2 = model(view1), model(view2)
# loss = info_nce_loss(z1, z2, temperature=0.5)
```

## 4. Evaluating Representation Quality: Linear Probing

To test whether pretraining learned genuinely useful features (rather than features that only
help the specific pretext task), **freeze** the pretrained encoder $f_\theta$ entirely and train
only a single linear classifier on top of its (frozen) output features, using a small labeled
dataset. High accuracy under this constrained protocol is strong evidence that the *frozen*
representation itself is linearly separable by class — i.e., that pretraining organized the
representation space in a semantically meaningful way, isolating representation quality from
"what a sufficiently powerful classifier could do with any features."

## 5. In-Class Exercise

For a batch of 2 images (4 augmented views total: $z_1^{(1)}, z_2^{(1)}, z_1^{(2)}, z_2^{(2)}$),
list every pair this batch's InfoNCE loss treats as a negative for the anchor $z_1^{(1)}$, and
identify its one positive.
