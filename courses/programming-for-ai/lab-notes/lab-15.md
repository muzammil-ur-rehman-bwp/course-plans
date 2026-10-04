# Lab Notes 15 — Training with PyTorch/Keras

**Concept recap:** `loss.backward()` performs backpropagation automatically; `optimizer.step()`
applies the weight update; always track both training and validation loss to catch overfitting
during training, not only after the fact.

**Common pitfalls:**
- Forgetting `optimizer.zero_grad()` before `loss.backward()` in PyTorch — gradients accumulate
  across batches by default, which silently corrupts training.
- Evaluating the model in training mode (dropout/batchnorm active) instead of switching to
  evaluation mode (`model.eval()`) before validation/test inference.
- Training for too few epochs to draw a conclusion, or too many without checking for overfitting
  along the way.

**Debugging tip:** if loss is `NaN`, suspect a learning rate that's too high, or unnormalized
input data — check both before anything else.

**Instructor tip:** this lab is the capstone-skills dry run — many capstone projects (Week 16)
will use this exact train/evaluate/plot pattern, so make sure every student leaves with a
working, understood template.
