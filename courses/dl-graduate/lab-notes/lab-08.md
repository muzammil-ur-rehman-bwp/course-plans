# Lab Notes 8 — A Graph Convolutional Network From Scratch

**Concept recap:** a GCN layer computes $\sigma(\tilde D^{-1/2}\tilde A\tilde D^{-1/2}H W)$; the
symmetric degree normalization keeps aggregated feature magnitudes comparable across nodes of
different degree.

**Common pitfalls:**
- **GCN normalization errors — forgetting degree normalization entirely** (using raw $A$ or
  $\tilde A$ directly as the aggregation matrix): high-degree nodes then accumulate
  systematically larger-magnitude features purely as an artifact of degree, not content, which
  can look like "the model trained" but generalizes poorly across nodes of varying degree.
- Forgetting to add the self-loop ($\tilde A = A+I$) — without it, a node's own features are
  excluded from its own update, discarding the node's own prior information at every layer.
- Computing $\tilde D^{-1/2}$ via elementwise `deg.pow(-0.5)` on a degree vector that contains a
  **zero** (an isolated node with no edges and no self-loop added yet) — this produces `inf`,
  propagating `NaN`s through the rest of the computation; always add self-loops *before* computing
  degree.
- Using a dense `(n,n)` adjacency matrix implementation (as in the lecture, for clarity) on a
  graph too large for dense storage to be practical — fine for this lab's toy graphs, but flag to
  students that production GNN code uses sparse operations (e.g., via `torch_geometric`) for
  exactly this reason.

**Debugging tip:** if training loss is `NaN` from the first step, print `deg` (the degree vector)
immediately after adding self-loops and check for any exact zero before looking anywhere else.

**Instructor tip:** Task D's self-loop ablation usually shows a real, visible accuracy drop even
on small toy graphs — use it to make the "why self-loops matter" argument concrete rather than
purely abstract.
