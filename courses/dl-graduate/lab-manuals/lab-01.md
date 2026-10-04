# Lab Manual 1 — Environment Setup and Prerequisite Fluency Warm-Up

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Set up the semester's PyTorch/torchvision/`torch_geometric` environment, and confirm fluency
with the assumed undergraduate prerequisite material by reimplementing a known small CNN.

## Setup
Create `lab01.ipynb` in Colab (or local Jupyter with a PyTorch install). Verify GPU availability.

## Procedure
1. **Task A — Environment check:** print `torch.__version__`, `torchvision.__version__`, and
   `torch.cuda.is_available()`; attempt to import `torch_geometric` and note whether it succeeds
   (a from-scratch fallback is provided in Weeks 8–9 if not).
2. **Task B — Prerequisite warm-up:** reimplement a small CNN (2 conv layers + 1 FC layer) for
   Fashion-MNIST classification from memory, using only what was covered in *Introduction to Deep
   Learning* (no advanced architectures yet); train for 2 epochs and report test accuracy.
3. **Task C — Topic mapping:** given 8 short topic/paper-title phrases (provided in the
   notebook), write which week of this course's syllabus each belongs to and one justifying
   sentence.
4. **Task D — Scope check:** in a markdown cell, write 3 sentences distinguishing what this course
   assumes (undergraduate survey, tabular RL) from what it will teach at depth.

## Expected Output
A notebook with Tasks A–D; Task B's trained model and test accuracy printed.

## Submission
Submit `lab01.ipynb` by the end of the lab session.
