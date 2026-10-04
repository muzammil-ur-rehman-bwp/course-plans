# Lab Manual 12 — Mixed Precision, Gradient Checkpointing, and LoRA

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement a mixed-precision training loop, a gradient-checkpointed block, and a LoRA-wrapped
linear layer, and measure their practical effects.

## Setup
Create `lab12.ipynb`. Start from the lecture's `mixed_precision_step`, `CheckpointedBlock`, and
`LoRALinear`. GPU required for meaningful timing/memory comparisons (Colab GPU runtime).

## Procedure
1. **Task A — Mixed precision:** train a small model with and without `autocast`/`GradScaler`;
   compare training step time and (if on GPU) peak memory via `torch.cuda.max_memory_allocated()`.
2. **Task B — Gradient checkpointing:** build a deeper stack of `CheckpointedBlock`s vs. plain
   (non-checkpointed) equivalent blocks; compare peak memory and step time between the two.
3. **Task C — LoRA:** wrap a frozen `nn.Linear(512, 512)` with `LoRALinear` at rank $r=8$; confirm
   only the LoRA parameters have `requires_grad=True`, and fine-tune on a small toy task; compare
   trainable parameter count against a full fine-tune of the same layer.
4. **Task D — Discussion:** in a markdown cell, state, for each technique, whether it primarily
   saves compute time, memory, or trainable-parameter count (storage), and why.

## Expected Output
A notebook with Tasks A–D; Task A/B's timing/memory comparisons and Task C's parameter-count
comparison printed.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
