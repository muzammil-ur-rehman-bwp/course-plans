# Week 14 Summary — Practical Deep Learning Workflows

**Key takeaways:**
- A full transfer-learning project chains data loading, augmentation, a pretrained backbone,
  fine-tuning, and evaluation into one repeatable workflow.
- Models are saved/loaded via `state_dict` checkpoints; TorchScript (tracing/scripting) and ONNX
  export are the conceptual next steps for deployment beyond a Python training script.
- Debugging a network that will not train follows a cheapest-check-first order: verify data/
  labels, confirm the model can overfit a single batch, check for a forgotten
  `model.train()`/`model.eval()` switch, inspect gradient norms, and sanity-check the learning
  rate.
- Every technique used in this workflow (augmentation, schedules, transfer learning, AdamW, label
  smoothing) was introduced in a prior week — this week assembles them, it does not introduce new
  theory.

**You should now be able to:** build an end-to-end transfer-learning workflow; save/reload a
trained model; diagnose and fix a broken training run using a structured checklist.

**Next week:** ethics and current trends — bias amplification, compute/environmental cost, and a
grounded survey of foundation models and self-supervised pretraining.
