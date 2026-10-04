# Week 13 Summary — Convolutional Neural Networks (Basics)

**Key takeaways:**
- Convolution slides a small, weight-shared kernel across the input, directly exploiting spatial
  locality that a flattened-input MLP discards entirely.
- Weight sharing means a convolutional layer's parameter count does not grow with image size,
  unlike a fully-connected layer on a flattened image.
- Max pooling downsamples feature maps and gives robustness to small translations within each
  pooling window.
- A minimal CNN (conv → pool → conv → pool → dense) trains with the exact same autograd-based
  loop from Week 12 — only the architecture changed, not the training machinery.
- This week is deliberately brief; full CNN architectural depth belongs to a dedicated Deep
  Learning course.

**You should now be able to:** compute a convolution and a max-pool operation by hand; explain
why CNNs are more parameter-efficient and better suited to image data than MLPs; build and train a
small CNN with a framework.

**Next week:** recurrent neural networks — the architecture family for sequential data, where
order and memory matter instead of spatial locality.
