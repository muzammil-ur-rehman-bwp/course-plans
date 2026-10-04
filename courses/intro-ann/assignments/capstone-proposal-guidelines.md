# Capstone Project — Proposal Guidelines

**Due:** Week 11 | **Weight:** 10% of the Capstone grade (20% total course weight)

## What to Submit
A 1-page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Problem statement**: what are you classifying/predicting, and on what kind of data (image,
   tabular, or small sequence data)?
2. **Dataset**: source, size, and format (must be finalized/accessible by the proposal deadline —
   e.g., MNIST, Fashion-MNIST, or another small, publicly available dataset; no "to be determined"
   datasets).
3. **Planned architecture**: a feedforward network (MLP), or, if appropriate to the data, a small
   CNN — built with the framework introduced in Week 12 (PyTorch or Keras).
4. **Required ablation/comparison**: state which single design choice you will isolate and
   compare (e.g., with vs. without dropout, or optimizer A vs. optimizer B), holding everything
   else fixed between the two runs.
5. **Evaluation plan**: how will you measure and report success (accuracy, loss curves,
   confusion matrix, or another metric appropriate to the task)?
6. **Team**: individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
use techniques covered in this course (MLP, optimizers, regularization, and the Week 12 framework;
a CNN is allowed given Week 13's material, but full architectures beyond the syllabus require
instructor pre-approval).

## Example Topics (for inspiration, not a closed list)
An MNIST or Fashion-MNIST digit/garment classifier with a dropout ablation; a small tabular-data
MLP (e.g., predicting a binary outcome from a public tabular dataset) comparing Adam vs. SGD with
momentum; a small CNN vs. an MLP comparison on the same image dataset, holding parameter count
roughly fixed.
