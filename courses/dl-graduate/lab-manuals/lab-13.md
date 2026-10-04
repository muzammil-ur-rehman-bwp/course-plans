# Lab Manual 13 — Estimating Pretraining Compute and a CLIP-Style Loss Sketch

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Estimate relative pretraining compute from parameter/data scale, and implement a CLIP-style
contrastive image-text loss.

## Setup
Create `lab13.ipynb`. Start from the lecture's `rough_pretraining_compute` and
`clip_style_loss`.

## Procedure
1. **Task A — Compute estimation:** for 4 provided (parameter count, training token count) pairs
   spanning several orders of magnitude, compute relative pretraining FLOPs and rank them.
2. **Task B — CLIP-style loss:** implement `clip_style_loss`; on a toy batch of random
   "image" and "text" embedding vectors (standing in for real encoder outputs), verify the loss
   decreases when a batch's embeddings are modified so that matching pairs are made more similar
   to each other than to mismatched pairs.
3. **Task C — Zero-shot sketch:** given one toy image embedding and 3 candidate toy "class
   description" text embeddings, compute cosine similarities and identify the predicted class
   (highest similarity) — a minimal sketch of CLIP-style zero-shot classification.
4. **Task D — Discussion:** in a markdown cell, using Task A's numbers, discuss what the compute
   ratio implies about who can feasibly pretrain each configuration from scratch.

## Expected Output
A notebook with Tasks A–D; Task A's ranked compute table and Task C's predicted class printed.

## Submission
Submit `lab13.ipynb` by the end of the lab session.
