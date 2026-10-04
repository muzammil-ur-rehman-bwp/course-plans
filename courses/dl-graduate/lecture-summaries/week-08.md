# Week 8 Summary — Graph Neural Networks I; Midterm Review

**Key takeaways:**
- Graphs are represented via adjacency structure plus node (and optionally edge) feature
  matrices; any learned operation must respect permutation invariance.
- The message-passing framework (aggregate-and-update) generalizes across most GNN variants.
- The GCN layer is $H^{(\ell+1)}=\sigma(\tilde D^{-1/2}\tilde A\tilde D^{-1/2}H^{(\ell)}W^{(\ell)})$;
  the symmetric degree normalization keeps aggregated magnitudes comparable across nodes of
  different degree.

**You should now be able to:** implement a GCN layer from scratch and stack two layers for node
classification on a small graph.

**Next week:** Midterm Exam, then graph neural networks II — GraphSAGE and Graph Attention
Networks.
