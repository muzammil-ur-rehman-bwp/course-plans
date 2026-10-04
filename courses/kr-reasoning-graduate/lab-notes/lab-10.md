# Lab Notes 10 — TransE Knowledge-Graph Embedding

**Concept recap:** TransE embeds entities/relations so that h + r ≈ t for a valid triple;
trained via a margin-based ranking loss between a positive triple and a corrupted negative one.

**Common pitfalls:**
- **Scale/normalization issues**: TransE conventionally keeps entity (and often relation)
  embeddings normalized to the unit sphere after every gradient step — skipping this lets
  embeddings grow unboundedly, which can trivially "solve" the training loss by making every
  vector huge rather than learning a meaningful relative structure; always renormalize as shown
  in `train_transe`.
- **Corrupting into an accidental true triple**: `corrupt` should, strictly, avoid producing a
  corrupted triple that happens to already be a true fact in the graph (a "false negative") —
  for the toy scale in this lab this is rare but worth checking for in Task B if training loss
  behaves strangely.
- **Using different norms inconsistently**: the scoring function and the training loss must use
  the *same* norm (L1 or L2) — mixing them silently makes the margin comparison meaningless.
- In Task D, expecting a tiny toy graph to show a strong, noise-free trend across dimension/
  margin settings — at this scale, some run-to-run variance (from random initialization) is
  expected; report the trend honestly rather than overclaiming a clean monotonic pattern from a
  single run per setting.

**Debugging tip:** before trusting Task C's link-prediction ranking, print the average positive
vs. negative triple distances from Task B — if positive distances are not noticeably smaller
than negative ones after training, the model has not learned anything yet and link prediction
results should not be trusted.

**Instructor tip:** have students compute the TransE score by hand for one toy triple with
hand-picked 2-D vectors before running any training — seeing the geometric "translation" picture
(h + r should land near t) makes the abstract norm formula concrete.
