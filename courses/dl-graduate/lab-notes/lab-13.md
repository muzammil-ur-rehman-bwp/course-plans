# Lab Notes 13 — Estimating Pretraining Compute and a CLIP-Style Loss Sketch

**Concept recap:** rough pretraining compute scales as roughly $6\times\text{params}\times
\text{tokens}$; a CLIP-style loss is InfoNCE applied bidirectionally across paired image/text
embeddings.

**Common pitfalls:**
- Comparing two model configurations' compute estimates without matching **units** (e.g., one
  parameter count in millions, another in raw units) — always print raw unscaled numbers before
  computing a ratio.
- In `clip_style_loss`, forgetting to normalize **both** `image_embeds` and `text_embeds` before
  computing the similarity matrix — an unnormalized modality on either side reintroduces the
  magnitude-confound problem from Week 5's InfoNCE loss.
- Confusing the loss's two directions (image→text and text→image) — forgetting the `.T` on the
  second `cross_entropy` call computes the same direction twice rather than the symmetric
  objective, which is a silent, non-crashing bug.
- In Task C's zero-shot sketch, comparing **unnormalized** toy embeddings via raw dot product
  instead of cosine similarity — magnitude differences between the toy image and text vectors can
  then dominate the "similarity" ranking in a way unrelated to actual semantic alignment.

**Debugging tip:** if Task B's sanity check doesn't show a loss decrease when matching pairs are
made more similar, verify normalization on both embedding sets independently before checking the
loss's indexing/targets.

**Instructor tip:** Task D's discussion often undersells the compute-concentration point if
students only compare raw FLOP numbers — explicitly ask them to translate the ratio into a
real-world comparison (e.g., "this is the difference between a single GPU for a weekend versus a
large cluster for months") to make the implication concrete.
