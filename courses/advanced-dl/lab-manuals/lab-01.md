# Lab Manual 1 — Environment Setup and Landscape Mapping

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Verify the PyTorch/GPU environment used throughout the semester; self-assess prerequisite
fluency against the graduate *Deep Learning* checklist; practice mapping a paper title to the
correct course/week.

## Setup
1. Create a fresh virtual environment (conda or venv) with Python 3.10+.
2. Install PyTorch (CPU or CUDA build as available) and Matplotlib.
3. Create `lab01.ipynb`.

## Procedure
1. **Task A — Environment check:** print the PyTorch version and confirm GPU availability
   (`torch.cuda.is_available()`); if no GPU is available, confirm the notebook still runs on CPU
   (every lab this semester is designed to run, if more slowly, on CPU).
2. **Task B — Prerequisite self-assessment:** for each of the seven graduate-course checklist
   items (Transformer from scratch, advanced CNNs, contrastive learning/InfoNCE, DDPM diffusion,
   GNNs, deep RL, large-scale training basics), write one sentence stating your current
   confidence level and, for any item below "comfortable," name the specific graduate-course
   resource you will review before it is needed.
3. **Task C — Landscape mapping:** given the five paper titles distributed in lecture, state in
   one sentence each which course (this one, or a named sibling postgraduate course) would own
   the paper's central contribution, and the specific week/pillar it falls under.
4. **Task D — Research-interest paragraph:** write a short (100–150 word) paragraph on which of
   this course's eight topic areas (SDE diffusion, advanced diffusion, MoE, ICL mechanics,
   RLHF/DPO, efficient inference, NAS, systems-scaling) you are currently most curious about, as
   an informal, non-binding first step toward the eventual capstone topic.

## Expected Output
A notebook/document with four clearly labeled sections (A–D), each with correct, complete content.

## Submission
Submit `lab01.ipynb` (or an equivalent document) via the course submission system by the end of
the lab session.
