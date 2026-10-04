# Lab Manual 14 — End-to-End Transfer Learning Workflow and Debugging

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Assemble a full transfer-learning workflow end to end; save/reload the trained model; find and
fix the fault in a provided broken training script.

## Setup
Create `lab14.ipynb`. A separate provided file `lab14_broken.py` contains a deliberately broken
training script for Task C.

## Procedure
1. **Task A — End-to-end workflow:** assemble the full pipeline from lecture (augmented data
   loading → pretrained backbone → AdamW + cosine schedule with warmup → label smoothing →
   training with correct `model.train()`/`model.eval()` switching) on a provided image dataset.
2. **Task B — Save and reload:** save the trained model's `state_dict`; in a fresh cell (simulating
   a new session), rebuild the architecture and reload the checkpoint; confirm identical
   validation accuracy before and after reloading.
3. **Task C — Debug the broken script:** `lab14_broken.py` fails to train (loss stays flat).
   Apply the Week 14 checklist (data/label check → overfit-one-batch test → train/eval mode check
   → gradient-norm inspection → LR sanity check) in order, and document in markdown which step
   revealed the bug and what the bug was.
4. **Task D — Fix and verify:** apply the fix; retrain and confirm the loss now decreases as
   expected; report before/after loss curves.

## Expected Output
A notebook with Tasks A–D, the save/reload verification, and the documented debugging process and
fix for Task C/D.

## Submission
Submit `lab14.ipynb` by the end of the lab session.
