# Lab Manual 8 — Quantization and Knowledge Distillation

**Duration:** 3 hours | **Prerequisite:** Week 8 lecture

## Objectives
Implement post-training quantization and quantization-aware training, and the knowledge-
distillation loss, measuring the resulting accuracy tradeoffs.

## Setup
1. Reuse your Week 1 virtual environment.
2. Create `lab08.ipynb`.

## Procedure
1. **Task A — PTQ:** train a small network on a toy classification task; implement
   `ptq_quantize_per_channel` exactly as in the Week 8 lecture content and quantize its weights at
   $b \in \{8,4,2\}$ bits, reporting the accuracy drop at each bit-width.
2. **Task B — QAT:** implement `FakeQuantizeSTE` exactly as in lecture; retrain the same
   architecture from scratch with fake-quantized weights at $b=4$, and compare its final accuracy
   against Task A's plain PTQ result at $b=4$.
3. **Task C — Distillation:** train a larger "teacher" network to convergence on the same task;
   implement `distillation_loss` exactly as in lecture and train a smaller "student" network with
   it, and separately train the same student architecture from hard labels alone.
4. **Task D — Synthesis table:** report a single table comparing accuracy, approximate model
   size, and (for the distillation case) parameter count across: full-precision baseline, PTQ at
   $b=4$, QAT at $b=4$, hard-label student, distilled student.

## Expected Output
A notebook with four clearly labeled sections (A–D) and the Task D synthesis table.

## Submission
Submit `lab08.ipynb` via the course submission system by the end of the lab session.
