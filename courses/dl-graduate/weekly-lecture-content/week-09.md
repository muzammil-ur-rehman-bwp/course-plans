# Week 9 — Lecture Content: Graph Neural Networks II (Post-Midterm)

## 1. GraphSAGE: Neighborhood Sampling and Aggregation

The GCN layer (Week 8) aggregates over a node's *entire* neighborhood, which becomes expensive on
large graphs with high-degree "hub" nodes. **GraphSAGE** addresses scalability by (a) **sampling**
a fixed-size subset of each node's neighbors at each layer (rather than using all of them), and
(b) aggregating the sampled neighbors' representations with a chosen aggregator (mean, max-pool,
or an LSTM over a random neighbor ordering), then **concatenating** the aggregated neighbor
representation with the node's own previous representation before a linear transform:

$$
h_v^{(\ell)} = \sigma\Big(W^{(\ell)} \cdot \text{CONCAT}\big(h_v^{(\ell-1)},\
\text{AGGREGATE}\big(\{h_u^{(\ell-1)} : u \in \mathcal{N}_{\text{sample}}(v)\}\big)\big)\Big).
$$

Concatenating (rather than only summing, as in a plain GCN update) explicitly preserves the
node's own prior representation as a separate signal from its neighbors' aggregated signal.
Fixed-size neighbor sampling bounds each layer's per-node computation independent of the actual
(possibly very large) degree, which is what makes GraphSAGE practical on graphs with millions of
nodes.

```python
import torch
import torch.nn as nn

class GraphSAGELayer(nn.Module):
    def __init__(self, in_features, out_features):
        super().__init__()
        self.linear = nn.Linear(2 * in_features, out_features)

    def forward(self, X, A, sample_size=None):
        n = A.size(0)
        agg = torch.zeros_like(X)
        for v in range(n):
            neighbors = A[v].nonzero(as_tuple=True)[0]
            if sample_size is not None and len(neighbors) > sample_size:
                idx = torch.randperm(len(neighbors))[:sample_size]
                neighbors = neighbors[idx]
            if len(neighbors) > 0:
                agg[v] = X[neighbors].mean(dim=0)       # mean aggregator
        combined = torch.cat([X, agg], dim=1)
        return torch.relu(self.linear(combined))
```

## 2. Graph Attention Networks (GAT)

A GCN/GraphSAGE layer weighs neighbors by a *fixed* rule (degree normalization, or a plain mean) —
every neighbor of a given node is treated identically (or near-identically) regardless of how
relevant it actually is. **Graph Attention Networks** instead *learn* a per-neighbor attention
weight, following the same softmax-normalized attention idea as Week 3, now applied over a node's
graph neighbors instead of over sequence positions. For node $v$ and neighbor $u$, compute an
unnormalized attention score from their (linearly transformed) features:

$$
e_{vu} = \text{LeakyReLU}\big(a^\top [Wh_v \,\|\, Wh_u]\big),
$$

where $W$ is a shared learned linear transform, $a$ is a learned attention vector, and $\|$
denotes concatenation. Normalize across $v$'s neighbors with softmax,

$$
\alpha_{vu} = \frac{\exp(e_{vu})}{\sum_{k\in\mathcal N(v)}\exp(e_{vk})},
$$

and aggregate as an attention-weighted sum (optionally followed by a nonlinearity, and in practice
computed with multiple attention heads concatenated or averaged, exactly as in Week 3):

$$
h_v' = \sigma\Big(\sum_{u\in\mathcal N(v)} \alpha_{vu}\, Wh_u\Big).
$$

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class GATLayer(nn.Module):
    def __init__(self, in_features, out_features):
        super().__init__()
        self.W = nn.Linear(in_features, out_features, bias=False)
        self.a = nn.Parameter(torch.empty(2 * out_features))
        nn.init.xavier_uniform_(self.a.view(1, -1))

    def forward(self, X, A):
        n = X.size(0)
        Wh = self.W(X)                                              # (n, out_features)
        scores = torch.full((n, n), float("-inf"))
        for v in range(n):
            for u in range(n):
                if A[v, u] > 0 or u == v:
                    pair = torch.cat([Wh[v], Wh[u]])
                    scores[v, u] = F.leaky_relu(pair @ self.a, negative_slope=0.2)
        alpha = F.softmax(scores, dim=1)                              # row-wise softmax over neighbors
        return F.elu(alpha @ Wh)
```

## 3. Real Applications (Conceptual)

- **Molecule property prediction:** atoms are nodes (with element-type features), bonds are
  edges (with bond-order/type features); message passing lets each atom's representation
  incorporate its local chemical neighborhood, supporting prediction of molecular properties
  (e.g., solubility, toxicity) from the graph structure alone.
- **Recommendation systems:** users and items form a bipartite graph (user–item interaction
  edges); GNN-based aggregation propagates information across this bipartite structure so a
  user's representation reflects the items they have interacted with, and vice versa, supporting
  link prediction (recommending unseen edges).

## 4. In-Class Exercise

For a node $v$ with 3 neighbors and raw GAT scores $e_{v1}=2.0$, $e_{v2}=1.0$, $e_{v3}=0.5$,
compute the softmax-normalized attention weights $\alpha_{v1},\alpha_{v2},\alpha_{v3}$ by hand.
