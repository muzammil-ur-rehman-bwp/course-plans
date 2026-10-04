# Lab Manual 5 — Augmentation, LR Schedules, and Transfer Learning

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Add a data augmentation pipeline and a learning rate schedule to a CNN; fine-tune a pretrained
ResNet on the same dataset and compare against training from scratch.

## Setup
Create `lab05.ipynb`. Reuse Lab 4's dataset.

## Procedure
1. **Task A — Augmentation:** add a `torchvision.transforms` augmentation pipeline (random crop,
   horizontal flip) to the training set only; retrain Lab 4's best CNN and compare
   training/validation curves against the unaugmented version.
2. **Task B — LR schedule:** add a `CosineAnnealingLR` schedule (with a short linear warmup) to
   Task A's training run; compare convergence speed and stability against a fixed learning rate.
3. **Task C — Transfer learning:** load `torchvision.models.resnet18(weights=...)`, replace the
   final layer for your number of classes, and fine-tune on the same dataset (feature extraction
   first, then full fine-tuning if time allows).
4. **Task D — Three-way comparison:** report test accuracy, parameter count, and training time for
   (i) Lab 4's from-scratch CNN, (ii) Task B's augmented/scheduled version, and (iii) Task C's
   fine-tuned ResNet, in one table.

## Expected Output
A notebook with Tasks A–D and the final three-way comparison table.

## Submission
Submit `lab05.ipynb` by the end of the lab session.
