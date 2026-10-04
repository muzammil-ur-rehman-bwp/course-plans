# Lab Manual 5 — Contrastive Representation Learning with InfoNCE

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement the InfoNCE loss and a minimal SimCLR-style training step; evaluate the learned
representation via linear probing.

## Setup
Create `lab05.ipynb`. Start from the lecture's `info_nce_loss` and `SimCLRModel`. Use a small
image dataset (CIFAR-10 subset or Fashion-MNIST) with `torchvision.transforms` augmentations.

## Procedure
1. **Task A — InfoNCE loss:** implement `info_nce_loss`; on a toy batch of 4 "images" (random
   tensors standing in for two augmented pairs), verify the loss decreases when you replace a
   negative with an exact copy of the positive (a sanity check that the loss rewards similarity
   to the true positive).
2. **Task B — SimCLR pretraining:** build `SimCLRModel` with a small CNN encoder; pretrain for a
   modest number of epochs (no labels) using two-view augmentation and the InfoNCE loss; plot the
   training loss curve.
3. **Task C — Linear probing:** freeze the pretrained encoder; train only a linear classifier on
   top using a small labeled subset; report test accuracy.
4. **Task D — Baseline comparison:** train the same linear-classifier-on-frozen-features
   protocol on a **randomly initialized** (not pretrained) encoder; compare accuracy against
   Task C and discuss what the gap shows about the value of pretraining.

## Expected Output
A notebook with Tasks A–D; Task C vs. Task D accuracy reported side by side.

## Submission
Submit `lab05.ipynb` by the end of the lab session.
