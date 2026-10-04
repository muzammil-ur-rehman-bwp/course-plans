# Week 8 — Lecture Content: Graph Neural Networks I; Midterm Review

## 1. Representing Graphs for Learning

A graph $G=(V,E)$ is represented for learning by:
- An **adjacency matrix** $A \in \{0,1\}^{n\times n}$ ($A_{uv}=1$ iff an edge connects $u,v$) or an
  adjacency list for sparse graphs.
- A **node feature matrix** $X \in \mathbb{R}^{n\times d}$ (one feature vector per node).
- Optionally, **edge features** (e.g., edge type/weight) when edges carry information beyond
  connectivity.

Unlike an image (a regular grid) or a sequence (a linear order), a graph's connectivity is
irregular and has no canonical ordering of nodes — any learned operation must be invariant to how
nodes happen to be indexed (**permutation invariance/equivariance**).

## 2. The Message-Passing Framework

Almost every GNN variant follows the same **aggregate-and-update** pattern at each layer $\ell$:

$$
m_{u\to v}^{(\ell)} = \text{MESSAGE}\big(h_u^{(\ell-1)}, h_v^{(\ell-1)}, e_{uv}\big), \qquad
h_v^{(\ell)} = \text{UPDATE}\Big(h_v^{(\ell-1)},\ \text{AGGREGATE}\big(\{m_{u\to
v}^{(\ell)} : u\in\mathcal N(v)\}\big)\Big),
$$

i.e., every node computes a "message" to send along each incoming edge, aggregates the messages
from its neighbors with a permutation-invariant function (sum/mean/max), and updates its own
representation from its previous representation plus the aggregated message. Stacking $L$ layers
lets information from a node's $L$-hop neighborhood reach it — analogous to receptive-field growth
in stacked CNN layers.

## 3. The Graph Convolutional Network (GCN) Layer

**Spectral motivation (brief).** Convolution on a graph can be defined via the eigendecomposition
of the graph Laplacian (the analogue of the Fourier basis for grid/sequence convolution). Directly
computing this is expensive; the GCN layer uses a well-known first-order (localized)
approximation of this spectral convolution, which turns out to have the simple spatial form below.

**Practical spatial form.** Let $A$ be the adjacency matrix, $\tilde A = A + I$ (adding
self-loops, so a node's own features are included in its own update), and $\tilde D$ the diagonal
degree matrix of $\tilde A$ ($\tilde D_{ii} = \sum_j \tilde A_{ij}$). The GCN layer is

$$
H^{(\ell+1)} = \sigma\!\left(\tilde D^{-1/2}\tilde A\tilde D^{-1/2} H^{(\ell)} W^{(\ell)}\right).
$$

**Why the symmetric degree normalization.** Without normalization, summing a high-degree node's
many neighbor features produces systematically larger-magnitude aggregated vectors than a
low-degree node's, confounding "this node has an unusually large representation" with "this node
simply has many neighbors." The symmetric normalization $\tilde D^{-1/2}\tilde A\tilde D^{-1/2}$
rescales each edge's contribution by $1/\sqrt{\deg(u)\deg(v)}$, keeping aggregated magnitudes
comparable across nodes of different degree and matching the normalization that falls out of the
underlying spectral derivation.

```python
import torch
import torch.nn as nn

class GCNLayer(nn.Module):
    def __init__(self, in_features, out_features):
        super().__init__()
        self.linear = nn.Linear(in_features, out_features)

    def forward(self, X, A):
        n = A.size(0)
        A_tilde = A + torch.eye(n, device=A.device)
        deg = A_tilde.sum(dim=1)
        D_inv_sqrt = torch.diag(deg.pow(-0.5))
        A_norm = D_inv_sqrt @ A_tilde @ D_inv_sqrt
        return torch.relu(self.linear(A_norm @ X))

class TwoLayerGCN(nn.Module):
    def __init__(self, in_features, hidden, num_classes):
        super().__init__()
        self.gc1 = GCNLayer(in_features, hidden)
        self.gc2 = GCNLayer(hidden, num_classes)

    def forward(self, X, A):
        h = self.gc1(X, A)
        return self.gc2(h, A)   # logits, one row per node

# toy 4-node graph: a path 0-1-2-3
A = torch.tensor([[0,1,0,0],[1,0,1,0],[0,1,0,1],[0,0,1,0]], dtype=torch.float32)
X = torch.eye(4)                 # one-hot node features as a toy input
model = TwoLayerGCN(in_features=4, hidden=8, num_classes=2)
logits = model(X, A)
print(logits.shape)              # torch.Size([4, 2]) -- one prediction per node
```

## 4. Midterm Review (Weeks 1–8)

Topics covered: advanced CNNs (ResNet residual formulation, DenseNet, depthwise separable
convolutions); the Transformer from scratch (scaled dot-product attention, multi-head attention,
positional encoding, encoder-decoder, layer-norm placement); Transformer variants (BERT-style
MLM, GPT-style causal LM, ViT); self-supervised/contrastive learning (InfoNCE, SimCLR framing,
linear probing); normalizing flows (change of variables) and the diffusion forward process; the
diffusion reverse process, simplified training loss, and sampling; graph representation,
message passing, and the GCN layer. Expect derivation, implementation-reasoning, and
short-answer/comparison questions in the same style as the lecture's in-class exercises.

## 5. In-Class Exercise

For the toy 4-node path graph above, compute $\tilde D^{-1/2}\tilde A \tilde D^{-1/2}$ by hand for
node 1 (degree 2 after adding the self-loop: degree 3) and confirm it matches the code's output.
