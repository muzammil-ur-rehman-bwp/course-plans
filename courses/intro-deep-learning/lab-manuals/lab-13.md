# Lab Manual 13 — AdamW, Label Smoothing, and Mixed Precision

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Compare `Adam` with an L2 penalty against `AdamW`; implement label smoothing; describe a
mixed-precision training loop conceptually (and run it if a GPU is available).

## Setup
Create `lab13.ipynb`. Reuse a CNN/dataset from an earlier lab (e.g., Lab 4 or Lab 5).

## Procedure
1. **Task A — Adam+L2 vs. AdamW:** train the same model twice, once with
   `Adam(weight_decay=...)` and once with `AdamW(weight_decay=...)` using the same numeric value;
   compare final validation accuracy and the training loss curves.
2. **Task B — Label smoothing:** retrain with `nn.CrossEntropyLoss(label_smoothing=0.1)` instead
   of plain cross-entropy; compare validation accuracy and (if time allows) the model's predicted
   confidence distribution on correct vs. incorrect test predictions.
3. **Task C — Mixed precision (GPU) or conceptual walkthrough (CPU-only):** if a GPU is available,
   wrap the training loop with `torch.cuda.amp.autocast()` and `GradScaler` as in lecture and
   compare training time per epoch against full precision; if no GPU is available, write out the
   mixed-precision training loop code and explain, in a markdown cell, what each line is
   responsible for.
4. **Task D — Large-batch LR scaling:** retrain with batch size doubled and the learning rate
   scaled linearly (per lecture's rule); compare convergence against the original batch
   size/learning rate.

## Expected Output
A notebook with Tasks A–D and the required comparisons/discussion for each.

## Submission
Submit `lab13.ipynb` by the end of the lab session.
