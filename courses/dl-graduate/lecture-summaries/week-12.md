# Week 12 Summary — Large-Scale Training Practices

**Key takeaways:**
- Data parallelism splits batches across replicated models; model parallelism splits the model
  itself when it does not fit on one device; real systems often combine both.
- Mixed-precision training computes in FP16/BF16 for speed/memory; loss scaling prevents small
  FP16 gradients from underflowing to zero.
- Gradient checkpointing discards and recomputes activations to trade compute for memory.
- LoRA freezes a pretrained weight matrix and learns only a low-rank update $BA$, cutting
  trainable parameters by roughly $r(d+k)$ vs. $dk$.

**You should now be able to:** describe a mixed-precision training loop, implement gradient
checkpointing, and implement a LoRA-wrapped linear layer.

**Next week:** multimodal and foundation models — a grounded survey of vision-language models and
the pretrain-then-adapt paradigm at scale.
