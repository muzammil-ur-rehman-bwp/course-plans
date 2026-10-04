# Week 12 Lecture Plan — Deep Learning (Graduate)
## Topic: Large-Scale Training Practices

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Compare data parallelism and model parallelism conceptually. (*Analyze*)
2. Explain mixed-precision training and loss scaling, and implement a gradient-checkpointing
   example. (*Apply, Analyze*)
3. Evaluate LoRA as a parameter-efficient fine-tuning technique. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why single-GPU, full-precision, full-fine-tuning training stops scaling |
| 0:15–0:35 | Data vs. model parallelism | Batch-splitting + gradient sync, vs. layer/parameter-splitting across devices |
| 0:35–1:05 | Mixed-precision training | FP16/BF16 compute; master weights; loss scaling and why it is needed |
| 1:05–1:15 | Break | — |
| 1:15–1:35 | Gradient checkpointing | Recompute-vs-store trade-off; `torch.utils.checkpoint` |
| 1:35–2:00 | LoRA | Freezing $W$, learning $BA$; parameter savings; live-coding a LoRA-wrapped linear layer |

### Materials/Equipment
- Live-coding environment, PyTorch (`autocast`, `GradScaler`, `torch.utils.checkpoint`)
- Slide diagram: LoRA's low-rank update next to a frozen pretrained weight matrix

### Formative Check (in-class)
For a $1024\times1024$ weight matrix and a LoRA rank $r=8$, students compute the parameter count
of the full fine-tune vs. the LoRA update and the resulting reduction factor.

### Link to Lab/Assessment
Lab 12: Implementing mixed-precision training, a gradient-checkpointed block, and a LoRA-wrapped
linear layer in PyTorch (see `lab-manuals/lab-12.md`).
