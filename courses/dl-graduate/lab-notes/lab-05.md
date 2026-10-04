# Lab Notes 5 — Contrastive Representation Learning with InfoNCE

**Concept recap:** InfoNCE treats representation learning as a $(K{+}1)$-way classification
problem (one positive vs. $K$ negatives in a batch); SimCLR forms positives from two augmented
views of the same image; linear probing freezes the encoder and trains only a linear head.

**Common pitfalls:**
- Forgetting to **L2-normalize** embeddings before computing cosine similarity — without
  normalization, the "similarity" is an unnormalized dot product whose magnitude is confounded
  with embedding norm, not just direction, which destabilizes the temperature-scaled softmax.
- Forgetting to exclude self-similarity (`fill_diagonal_(-inf)` in the lecture's
  `info_nce_loss`) — without this, every anchor trivially "wins" by matching itself, and the loss
  collapses to a near-zero, uninformative value.
- Using the **same** augmentation parameters (or no augmentation at all) for both views — this
  produces two identical views, making the pretext task trivial and the learned representations
  far less useful (the model never has to learn invariance to anything).
- In Task D, comparing linear-probe accuracy on a pretrained vs. random encoder **with
  different linear-head training budgets** (e.g., more epochs for one than the other) — hold the
  linear-probe training protocol identical across both conditions, or the comparison is
  confounded.

**Debugging tip:** if Task A's sanity check (replacing a negative with a copy of the positive)
does not show a loss decrease, first verify embeddings are L2-normalized and the diagonal is
masked — both bugs independently can mask this expected effect.

**Instructor tip:** Task D is the lab's conceptual payoff — the random-encoder baseline makes
concrete exactly how much of the linear-probe accuracy is attributable to pretraining versus to
the linear classifier's own capacity on raw/convolutional features.
