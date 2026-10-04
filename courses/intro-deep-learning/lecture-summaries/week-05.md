# Week 5 Summary — Training Deep Networks at Scale

**Key takeaways:**
- Data augmentation (random crop, flip, color jitter) acts as a regularizer by exposing the model
  to label-preserving input variations it would not otherwise see.
- Learning rate schedules (step decay, cosine annealing) and warmup stabilize training —
  warmup specifically avoids destabilizing updates at the start of training with a high learning
  rate.
- Transfer learning reuses a pretrained backbone's learned features; feature extraction (frozen
  backbone) suits small datasets, while full fine-tuning suits larger ones.
- `torchvision.models` provides pretrained backbones (e.g., ResNet) that can be adapted to a new
  task by replacing the final classification layer.

**You should now be able to:** build an augmentation pipeline; apply an LR schedule with warmup;
fine-tune a pretrained ResNet and justify feature-extraction vs. fine-tuning choices by dataset
size.

**Next week:** sequence models I — why vanilla RNNs struggle with long sequences, and the LSTM and
GRU cells in depth.
