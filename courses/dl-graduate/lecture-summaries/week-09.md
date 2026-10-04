# Week 9 Summary — Midterm Exam; Graph Neural Networks II

**Key takeaways:**
- GraphSAGE samples a fixed-size neighborhood per node for scalability and concatenates
  aggregated-neighbor features with the node's own prior representation.
- Graph Attention Networks learn per-neighbor attention weights (softmax-normalized, as in
  Week 3's attention) rather than using a fixed aggregation rule.
- GNNs apply to real tasks such as molecule property prediction (atoms as nodes, bonds as edges)
  and recommendation (bipartite user-item graphs).

**You should now be able to:** implement GraphSAGE-style mean aggregation and GAT-style
attention aggregation, and compare GCN/GraphSAGE/GAT on the same small graph.

**Next week:** deep reinforcement learning I — Deep Q-Networks, extending tabular Q-learning to
function approximation.
