# Week 4 — Lecture Content: Mixture-of-Experts and Sparse Architectures

## 1. Motivation
In a standard Transformer block (graduate course), the feed-forward sublayer is a single dense
MLP applied identically to every token: its parameter count and its per-token compute cost are
locked together — doubling parameters means doubling FLOPs per token. **Mixture-of-Experts (MoE)**
breaks this coupling by replacing the single dense FFN with many smaller "expert" FFNs and routing
each token to only a few of them.

## 2. The MoE Layer
Given $N$ expert sub-networks $E_1,\dots,E_N$ (each typically an ordinary feed-forward block of
the same shape as the dense FFN it replaces) and a learned router $W_g \in \mathbb R^{N\times d}$:
```
g(x) = softmax(W_g x) ∈ R^N              # gate distribution over experts for token x
```
**Dense** MoE would compute $y = \sum_{i=1}^N g(x)_i\, E_i(x)$ — but this costs as much as $N$
dense FFN evaluations per token, defeating the purpose. **Sparse (top-$k$) routing** instead
selects only the $k$ highest-scoring experts (commonly $k=1$ or $k=2$) and renormalizes their
gate values:
```
y = Σ_{i ∈ top-k(g(x))}  g(x)_i · E_i(x)
```
Only $k$ of the $N$ experts are actually evaluated for each token.

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class Expert(nn.Module):
    def __init__(self, d_model, d_ff):
        super().__init__()
        self.net = nn.Sequential(nn.Linear(d_model, d_ff), nn.GELU(), nn.Linear(d_ff, d_model))

    def forward(self, x):
        return self.net(x)

class TopKMoE(nn.Module):
    def __init__(self, d_model, d_ff, n_experts, k=2):
        super().__init__()
        self.k = k
        self.n_experts = n_experts
        self.router = nn.Linear(d_model, n_experts, bias=False)
        self.experts = nn.ModuleList([Expert(d_model, d_ff) for _ in range(n_experts)])

    def forward(self, x):
        # x: (batch, seq, d_model) -> flatten tokens for routing
        shape = x.shape
        x_flat = x.reshape(-1, shape[-1])                      # (n_tokens, d_model)
        logits = self.router(x_flat)                           # (n_tokens, n_experts)
        gate_probs = F.softmax(logits, dim=-1)
        topk_vals, topk_idx = gate_probs.topk(self.k, dim=-1)   # (n_tokens, k) each
        topk_vals = topk_vals / topk_vals.sum(dim=-1, keepdim=True)  # renormalize

        out = torch.zeros_like(x_flat)
        for slot in range(self.k):
            expert_idx = topk_idx[:, slot]          # which expert each token uses in this slot
            weight = topk_vals[:, slot].unsqueeze(-1)
            for e in range(self.n_experts):
                mask = expert_idx == e
                if mask.any():
                    out[mask] += weight[mask] * self.experts[e](x_flat[mask])
        return out.reshape(shape), gate_probs
```
(The double loop above is pedagogical — a production implementation batches per-expert dispatch
with gather/scatter operations for efficiency — but it computes exactly the sparse top-$k$ MoE
forward pass defined above.)

## 3. Why Sparsity Decouples Parameters from Compute
Total parameter count scales with $N$ (every expert's weights are stored, even if unused for a
given token): $P_{\mathrm{MoE}} \approx N \cdot P_{\mathrm{expert}}$. Per-token compute scales
only with $k$: $C_{\mathrm{MoE}} \approx k \cdot C_{\mathrm{expert}}$ (plus the cheap router).
Holding $k$ fixed (e.g., $k=2$) while growing $N$ grows the model's total capacity — and hence,
empirically, what it can represent and memorize — **without growing the FLOPs required to process
any single token**. This is the central efficiency argument for MoE: it lets parameter count scale
largely independently of per-example compute, a qualitatively different scaling axis than a dense
model offers (where the only way to add parameters is to add compute).

## 4. Load Balancing
Without an explicit incentive, gradient descent on the router tends toward **router collapse**:
a few experts receive most tokens early in training (by chance or by a small early advantage),
receive more gradient signal as a result, improve faster, and are routed to even more — a
rich-get-richer dynamic that under-trains the remaining experts and wastes the capacity $N$ was
supposed to buy. The standard mitigation is an **auxiliary load-balancing loss** penalizing uneven
expert usage across a batch — e.g., the squared coefficient of variation of per-expert token
counts:
```python
def load_balancing_loss(gate_probs, topk_idx, n_experts):
    # fraction of tokens routed to each expert (hard counts)
    counts = torch.zeros(n_experts, device=gate_probs.device)
    for e in range(n_experts):
        counts[e] = (topk_idx == e).float().sum()
    frac = counts / counts.sum().clamp(min=1.0)
    mean_gate = gate_probs.mean(dim=0)             # average soft gate prob per expert
    # Shazeer-style importance * load balancing signal: encourages both soft and hard
    # assignment to be uniform across experts.
    return n_experts * (frac * mean_gate).sum()
```
added to the main training loss with a small coefficient. A complementary mitigation is **noisy
gating** — adding learned or fixed noise to the router logits before the top-$k$ selection during
training, which widens exploration across experts early on and reduces the odds of premature
collapse.

## 5. Placement in a Transformer
MoE conventionally replaces only the feed-forward sublayer of a Transformer block, leaving
self-attention dense — attention's parameters are shared across all tokens in a way that does not
benefit from per-token routing the same way a token-local FFN does, and keeping attention dense
avoids complicating the (already load-balancing-sensitive) routing story further.

## 6. In-Class/Lab Exercise
Instantiate `TopKMoE` with $N=8$, $k=2$ and a dense `Expert`-shaped FFN with matched per-token
FLOPs ($k=2$ experts' worth), on a toy classification task. Train both with and without
`load_balancing_loss`, and plot the resulting per-expert token-usage histogram in each condition —
confirm the auxiliary loss flattens an otherwise skewed histogram. Compare final accuracy and
total parameter count of the MoE model against a dense FFN model with $N=8\times$ the parameters
but $N\times$ the FLOPs, and a dense FFN model with $k=2\times$ the parameters (FLOP-matched).
