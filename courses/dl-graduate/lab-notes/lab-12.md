# Lab Notes 12 — Mixed Precision, Gradient Checkpointing, and LoRA

**Concept recap:** mixed precision computes in FP16/BF16 with loss scaling to prevent gradient
underflow; gradient checkpointing recomputes (rather than stores) activations to save memory;
LoRA freezes a pretrained matrix and learns only a low-rank update.

**Common pitfalls:**
- Running Task A/B's memory/timing comparisons on **CPU** — mixed precision and the
  memory-saving effect of gradient checkpointing are primarily meaningful on GPU; on CPU, expect
  no speedup and sometimes no meaningful memory difference, which is not a bug.
- Forgetting `scaler.update()` after `scaler.step(optimizer)` — this is required every iteration
  to adjust the scale factor; omitting it leaves the scale factor static, which can eventually
  cause overflow or underflow as training progresses.
- In the `LoRALinear` implementation, forgetting to set `requires_grad_(False)` on the frozen
  layer's weight **and** bias — if only the weight is frozen, the bias still accumulates
  gradients and updates, silently performing a partial, undocumented fine-tune in addition to the
  intended LoRA update.
- Comparing `CheckpointedBlock` vs. a plain block's step **time** without also comparing peak
  memory — gradient checkpointing's cost (recomputation) should show up as a *time* increase and
  a memory *decrease* together; seeing only one effect usually means the comparison setup (e.g.,
  batch size, depth) wasn't large enough to make the memory difference visible.

**Debugging tip:** if `torch.cuda.max_memory_allocated()` reports the same value with and without
checkpointing, call `torch.cuda.reset_peak_memory_stats()` between the two runs — otherwise the
"peak" reported is contaminated by the previous run's allocations.

**Instructor tip:** Task D's discussion (which technique saves time vs. memory vs. parameter
count) is the lab's key takeaway — explicitly tabulate it on the board: mixed precision saves
time and some memory; checkpointing trades time for memory; LoRA saves trainable-parameter
storage, not necessarily training time or memory.
