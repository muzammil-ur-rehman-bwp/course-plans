# Lab Notes 2 — Initialization, Batch Normalization, and Dropout

**Concept recap:** initialization scale affects early-training activation/gradient magnitude;
batch normalization uses batch statistics in train mode and running statistics in eval mode;
dropout zeroes units during training only.

**Common pitfalls:**
- Forgetting `model.train()`/`model.eval()` before the respective training/validation loop —
  this is the single most common bug this week, since `BatchNorm` and `Dropout` behave
  differently in each mode, and a model left in `eval()` mode will silently stop updating batch
  norm running statistics.
- Using `nn.BatchNorm1d` with a batch size of 1, which produces undefined/unstable batch
  statistics — ensure the batch size used for any `BatchNorm1d` test is greater than 1.
- Applying Xavier initialization to a ReLU network (or He initialization to a tanh network) and
  getting confused by the resulting instability — match the initialization to the activation
  function, as covered in lecture.
- Comparing Task A's three initialization runs with different random seeds for the data
  shuffling, which can confound the comparison — hold the seed fixed across the three runs.

**Debugging tip:** if a batch-normalized model trains well but validation accuracy collapses
right after switching to `model.eval()`, check whether `track_running_stats` was accidentally
disabled or whether too few training batches were seen to build reliable running statistics.

**Instructor tip:** have students explicitly print `model.training` before each phase of Task B
and C — making the train/eval flag visible catches the most common mistake before it propagates
into a confusing results discrepancy.
