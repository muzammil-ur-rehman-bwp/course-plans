# Week 4 Summary — Mixture-of-Experts and Sparse Architectures

**Key takeaways:**
- An MoE layer replaces a dense FFN with $N$ experts and a softmax router; top-$k$ sparse routing
  evaluates only $k$ experts per token.
- Parameter count scales with $N$; per-token compute scales only with $k$ — sparsity decouples
  total capacity from per-example compute, a scaling axis a dense model cannot offer.
- Without mitigation, routers collapse onto a few popular experts; an auxiliary load-balancing
  loss (and/or noisy gating) counteracts this.
- MoE conventionally replaces only the FFN sublayer in a Transformer block, leaving attention
  dense.

**You should now be able to:** write the MoE gating/routing equation and its top-$k$ sparse form;
implement a top-$k$ MoE layer and a load-balancing loss; analyze the parameter/compute decoupling
argument.

**Next week:** In-context learning mechanics — the empirical phenomenon, induction heads as a
well-supported mechanistic finding, and the more speculative implicit-gradient-descent analogy.
