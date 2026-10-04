# Week 5 Summary — Self-Supervised and Contrastive Representation Learning

**Key takeaways:**
- A pretext task derives training labels automatically from unlabeled data; solving it well
  requires learning generally useful features.
- The InfoNCE loss treats representation learning as classifying a positive pair against a batch
  of negatives — it is the categorical cross-entropy loss for that $(K{+}1)$-way problem.
- A SimCLR-style pipeline forms positive pairs from two augmented views of the same image and
  uses the rest of the batch as negatives.
- Linear probing (freezing the encoder, training only a linear classifier) isolates
  representation quality from classifier capacity.

**You should now be able to:** derive and implement the InfoNCE loss, build a minimal
SimCLR-style training step, and describe the linear-probing evaluation protocol.

**Next week:** advanced generative models I — normalizing flows and the diffusion forward
process.
