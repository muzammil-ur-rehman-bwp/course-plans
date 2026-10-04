# Lab Notes 9 — GraphSAGE and Graph Attention Networks

**Concept recap:** GraphSAGE concatenates a node's own representation with an aggregated
(sampled) neighbor representation; GAT learns softmax-normalized per-neighbor attention weights
instead of using a fixed aggregation rule.

**Common pitfalls:**
- In `GraphSAGELayer`, forgetting that the `Linear` layer's input dimension must be
  **`2 * in_features`** (concatenation doubles the dimension) — a mismatched `Linear` input size
  is the most common shape-error source in this lab.
- In `GATLayer`, computing the attention score `e_{vu}` only for actual edges but forgetting to
  include the **self-loop** (`u == v`) case, which (as in GCN) excludes a node's own features from
  its own updated representation unless handled explicitly, as the lecture's implementation does
  via `A[v, u] > 0 or u == v`.
- Applying softmax over the **wrong dimension** of the GAT score matrix (over all nodes in the
  graph, `dim=0`, instead of over each node's own neighbor row, `dim=1`) — this silently produces
  a globally-normalized rather than per-node-normalized attention distribution.
- Comparing GCN/GraphSAGE/GAT accuracy (Task C) with **different** numbers of training epochs or
  learning rates per model — hold training protocol fixed across all three for a fair comparison,
  exactly as emphasized for research-methods comparisons in Week 14.

**Debugging tip:** if GAT training loss is unusually unstable, check the softmax dimension first
— a `dim=0` vs. `dim=1` bug still produces *valid* probabilities (they still sum to something
plausible along the wrong axis), so it will not raise a shape error, only a conceptually wrong
and poorly-trained model.

**Instructor tip:** Task D's attention-weight inspection is most illustrative on a node with
clearly heterogeneous neighbors (e.g., one close-topic neighbor and one unrelated one, in a
citation-graph dataset) — pick such a node deliberately for the demo rather than a random one.
