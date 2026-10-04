# Lab Notes 5 — Augmentation, LR Schedules, and Transfer Learning

**Concept recap:** augmentation transforms apply only to the training set; a pretrained backbone's
early layers hold general-purpose features reusable across tasks.

**Common pitfalls:**
- Accidentally applying the training augmentation pipeline to the validation/test set as well,
  which makes evaluation non-deterministic and results hard to reproduce across runs.
- Forgetting to call `scheduler.step()` (and calling it in the wrong place — once per epoch for
  most schedulers, not once per batch, unless the scheduler is specifically designed for
  per-batch stepping).
- Normalizing input images with statistics that do not match what the pretrained ResNet expects —
  `torchvision.models` pretrained weights expect ImageNet normalization statistics; using
  different ones silently degrades transfer-learning performance without raising an error.
- Leaving `requires_grad = False` on the backbone after intending to switch from feature
  extraction to full fine-tuning in Task C — verify which parameters are actually trainable with
  `[p.requires_grad for p in model.parameters()]` before training.

**Debugging tip:** if the fine-tuned ResNet performs worse than the from-scratch CNN, first check
input normalization and image size (pretrained ResNets expect specific input preprocessing) before
concluding that transfer learning "did not work" for this dataset.

**Instructor tip:** Task D's three-way comparison table is the single most informative artifact in
this lab — have students present it in the next lecture's warm-up discussion.
