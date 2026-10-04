# Week 9 — Lecture Content: Speculative Decoding

*(Midterm Exam covers Weeks 1–8; this content covers the post-midterm lecture.)*

## 1. Why Autoregressive Decoding Is Latency-Bound
Generating $n$ tokens autoregressively from a large target model requires $n$ sequential forward
passes — each one waits on the previous token being sampled before it can even begin, since the
next input depends on it. This is fundamentally latency-bound (not throughput-bound): a single
forward pass over a short sequence rarely saturates a modern accelerator's compute, so most of
that pass's wall-clock cost is "wasted" relative to the hardware's peak throughput. **Speculative
decoding** exploits this by getting a cheap model to guess ahead, then checking the guesses with
one parallel forward pass of the expensive model.

## 2. The Mechanism
Maintain a small, fast **draft model** $q$ and the large **target model** $p$ we actually want
samples from.
1. The draft model proposes $k$ candidate next tokens $\hat x_1,\dots,\hat x_k$ autoregressively
   (cheap, since it is small).
2. The target model runs **once**, in parallel, over the whole proposed block (the original
   prefix plus all $k$ draft tokens), computing $p(\cdot)$ at every one of the $k$ positions in a
   single forward pass — the same total sequence length the target model would have processed
   token-by-token anyway, now batched into one call.
3. Starting from position 1, **accept** $\hat x_i$ with probability $\min\!\big(1,\,
   p(\hat x_i)/q(\hat x_i)\big)$. If accepted, move to position $i+1$ and repeat. At the **first
   rejection** (position $j$), discard $\hat x_j$ and every draft token after it, and sample a
   replacement token $x_j$ from the **residual distribution**
   $p_{\mathrm{res}}(x) \propto \max(0,\, p(x) - q(x))$ (renormalized to sum to 1).

## 3. The Correctness Argument, Verified
**Claim:** the output token at every position has marginal distribution *exactly* $p$ — not an
approximation — regardless of how good or bad the draft model $q$ is (a bad $q$ only hurts speed,
never output quality).

**Proof (modified rejection sampling).** Fix one position; let $X\sim q$ be the draft's proposed
token, accepted with probability $a(X) := \min(1, p(X)/q(X))$. The probability the *accepted*
output equals a specific token $y$ with $p(y)\ge q(y)$ (so $a(y)=1$) is:
```
P(output = y, accepted) = q(y) · a(y) = q(y) · 1 = q(y)
```
For $y$ with $p(y) < q(y)$ (so $a(y) = p(y)/q(y)$):
```
P(output = y, accepted) = q(y) · p(y)/q(y) = p(y)
```
So in both cases, $P(\text{output}=y,\ \text{accepted}) = \min(p(y), q(y))$. The overall
acceptance probability is $P(\text{accepted}) = \sum_y \min(p(y),q(y))$. On rejection (probability
$1-P(\text{accepted}) = \sum_y \max(0, p(y)-q(y)) = \sum_y (p(y)-q(y))_+$), we resample from
$p_{\mathrm{res}}(y) = (p(y)-q(y))_+ / \sum_{y'}(p(y')-q(y'))_+$. The total probability of
outputting $y$ (accepted *or* resampled) is:
```
P(output = y) = min(p(y), q(y))  +  P(rejected) · p_res(y)
             = min(p(y), q(y))  +  Σ_{y'}(p(y')-q(y'))_+ · (p(y)-q(y))_+ / Σ_{y'}(p(y')-q(y'))_+
             = min(p(y), q(y))  +  (p(y) - q(y))_+
```
If $p(y) \geq q(y)$: $\min(p,q)=q(y)$ and $(p(y)-q(y))_+ = p(y)-q(y)$, summing to $p(y)$. If
$p(y) < q(y)$: $\min(p,q)=p(y)$ and $(p(y)-q(y))_+ = 0$, summing to $p(y)$. **Either way,
$P(\text{output}=y) = p(y)$, exactly.** $\blacksquare$

This is the key engineering payoff: speculative decoding is a *sampling-equivalent* optimization,
not an approximation — it changes nothing about what gets generated, only how fast.

```python
import torch

def speculative_step(p_target, q_draft, draft_token):
    """p_target, q_draft: probability vectors (vocab,) at the current position.
    draft_token: int, the draft model's proposed token at this position.
    Returns (accepted: bool, output_token: int)."""
    accept_prob = min(1.0, (p_target[draft_token] / q_draft[draft_token]).item())
    if torch.rand(1).item() < accept_prob:
        return True, draft_token
    residual = torch.clamp(p_target - q_draft, min=0.0)
    residual = residual / residual.sum()
    resampled = torch.multinomial(residual, 1).item()
    return False, resampled

def speculative_decode_block(p_target_fn, q_draft_fn, prefix, k=4):
    """Greedy illustrative driver: draft proposes k tokens autoregressively, target scores the
    whole block in one call (simulated here with k separate calls, standing in for one batched
    forward pass), accept/reject/resample is applied left to right."""
    draft_tokens, draft_probs = [], []
    ctx = list(prefix)
    for _ in range(k):
        q = q_draft_fn(ctx)
        tok = torch.multinomial(q, 1).item()
        draft_tokens.append(tok); draft_probs.append(q)
        ctx = ctx + [tok]

    output = []
    ctx = list(prefix)
    n_accepted = 0
    for i, tok in enumerate(draft_tokens):
        p = p_target_fn(ctx)                      # target's distribution at this position
        accepted, out_tok = speculative_step(p, draft_probs[i], tok)
        output.append(out_tok)
        ctx = ctx + [out_tok]
        if accepted:
            n_accepted += 1
        else:
            break
    return output, n_accepted
```

## 4. Why This Speeds Up Generation
Each target-model forward pass now produces, in expectation, more than one accepted token
whenever the draft model agrees with the target reasonably often — the expected number of tokens
emitted per large-model call is $\mathbb E[\text{accepted} + 1]$ (the "+1" for the resampled or
bonus token), amortizing the large model's expensive forward-pass cost over multiple output
tokens instead of exactly one. The better the draft model approximates the target (higher
agreement), the more tokens are typically accepted per large-model call, and the larger the
wall-clock speedup — with the correctness guarantee of Section 3 holding regardless.

## 5. KV-Cache Management on Rejection
Autoregressive decoding caches each layer's key/value projections for every previously-generated
token (the **KV cache**) so each new step's attention does not recompute attention over the whole
prefix from scratch. Speculative decoding computes the target model's forward pass over the whole
speculative block at once, so its KV cache is populated for *all* $k$ positions provisionally —
but if a draft token at position $j$ is rejected, every cache entry for positions $\geq j$
(computed under the now-discarded continuation) must be rolled back to position $j{-}1$ before
generation resumes from the resampled token, since those cache entries reflect attention over a
sequence prefix that is no longer valid going forward.

## 6. In-Class/Lab Exercise
Using `speculative_step`, construct a toy 3-symbol vocabulary with hand-chosen $p,q$ and run
10,000 trials; tabulate the empirical output-token frequency and confirm it matches $p$ to within
sampling noise, verifying Section 3's proof empirically. Using `speculative_decode_block` with toy
categorical "models" standing in for draft/target, measure the average number of accepted tokens
per block across many runs for a draft model that agrees with the target often versus one that
disagrees often, and relate the difference to the speedup argument of Section 4.
