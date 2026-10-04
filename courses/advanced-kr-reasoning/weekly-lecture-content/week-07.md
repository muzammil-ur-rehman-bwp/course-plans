# Week 7 — Lecture Content: Neuro-Symbolic Integration I — Differentiable and Fuzzy Logic

## 1. Why Logic Needs to Be Made Differentiable
A classical Boolean formula's truth value is a step function of its inputs: nudging an input
very slightly either changes nothing or flips the output discontinuously. Gradient-based
learning (the mechanism behind virtually all modern trainable models) needs a **useful gradient**
— a smooth signal saying "which direction to nudge this parameter to do slightly better." If a
symbolic rule's satisfaction is used as part of a training signal, it must be relaxed into a
smooth, real-valued function to be learnable by gradient descent at all.

## 2. t-Norms and t-Conorms
A **t-norm** generalizes conjunction to [0,1]-valued "truth degrees": any function
`T: [0,1]² → [0,1]` that is commutative, associative, monotonic in both arguments, and has 1 as
identity (`T(1,a) = a`). Three standard choices:
| t-norm | Formula | Note |
|---|---|---|
| Product | `T(a,b) = a·b` | smooth, nonzero gradient everywhere a,b > 0 |
| Gödel (min) | `T(a,b) = min(a,b)` | gradient is 0 w.r.t. the non-minimal argument |
| Łukasiewicz | `T(a,b) = max(0, a+b-1)` | piecewise-linear; zero gradient once clamped at 0 |

The matching **t-conorm** (generalizing disjunction) is obtained by De Morgan duality,
`S(a,b) = 1 - T(1-a, 1-b)`; for the product t-norm this gives the **probabilistic sum**
`S(a,b) = a + b - a·b`. **Negation** is standardly relaxed as `¬a = 1 - a`. A soft implication can
be built as `a → b := S(¬a, b)` (for a chosen t-conorm), or directly via the Łukasiewicz
implication `min(1, 1 - a + b)` (the same formula used numerically in Week 3's Ł3 logic — not a
coincidence: Łukasiewicz's three-valued logic *is* the T=1,F=0,U=0.5 restriction of this
continuous-valued system).

```python
import numpy as np

def product_tnorm(a, b):  return a * b
def godel_tnorm(a, b):    return np.minimum(a, b)
def luk_tnorm(a, b):      return np.maximum(0.0, a + b - 1.0)

def product_tconorm(a, b): return a + b - a * b
def godel_tconorm(a, b):   return np.maximum(a, b)
def luk_tconorm(a, b):     return np.minimum(1.0, a + b)

def soft_not(a): return 1.0 - a
```

## 3. Why the Product t-Norm Is Preferred for Learning
Consider `T(a,b)` with a=0.9 (close to true) and b=0.2 (close to false).
- Product: `T = 0.18`, and `∂T/∂a = b = 0.2` — **nonzero**: increasing a still moves the
  conjunction's value, so gradient descent gets a signal to push a and b both toward improving
  the conjunction.
- Gödel: `T = min(0.9, 0.2) = 0.2`, and `∂T/∂a = 0` (a is not the minimizer) — the gradient
  w.r.t. a is **exactly zero almost everywhere** whenever a ≠ b, so a parameter feeding into a
  loses all learning signal from this conjunction the moment it is not the bottleneck term. This
  is a real, practical liability, not a minor technicality: in a system with many conjoined soft
  constraints, most terms will not be the strict minimizer at any given training step, and the
  Gödel t-norm would starve most of them of gradient most of the time.

This is exactly why the product t-norm is the standard default in practical differentiable-logic
systems (e.g., the Logic Tensor Networks line of work), despite the Gödel t-norm's cleaner
correspondence to classical min/max reasoning.

## 4. A Differentiable Soft-Logic Loss in PyTorch
```python
import torch

def soft_implies(a, b):
    return torch.clamp(1.0 - a + b, max=1.0)  # Łukasiewicz implication

def constraint_loss(p_scores, q_scores):
    """Soft version of the universally-quantified rule ∀x. P(x) -> Q(x), given per-instance
    truth-degree scores p_scores, q_scores in [0,1] (e.g. sigmoid outputs of a model).
    Loss is 1 - mean soft-truth-value of the implication across instances."""
    truth = soft_implies(p_scores, q_scores)
    return 1.0 - truth.mean()

torch.manual_seed(0)
p_logits = torch.randn(8, requires_grad=True)
q_logits = torch.randn(8, requires_grad=True)
optimizer = torch.optim.SGD([p_logits, q_logits], lr=0.5)

for step in range(50):
    optimizer.zero_grad()
    p_scores = torch.sigmoid(p_logits)
    q_scores = torch.sigmoid(q_logits)
    loss = constraint_loss(p_scores, q_scores)
    loss.backward()
    optimizer.step()
    if step % 10 == 0:
        print(step, loss.item())
# loss should decrease toward 0 as q_scores are pulled up (or p_scores pulled down)
# wherever the implication is currently poorly satisfied
```
Running this shows `loss` decreasing over training steps: gradient descent is, in a very literal
sense, *learning to better satisfy a symbolic rule*, purely from the rule's differentiable
relaxation — no labeled examples of "P(x) → Q(x) is true" were ever provided; the constraint
itself supplied the training signal.

## 5. In-Class/Lab Exercise
Compute, by hand, `product_tnorm(0.9, 0.2)` and `godel_tnorm(0.9, 0.2)`, confirm they agree
(both 0.18... no — confirm they *disagree*: product gives 0.18, Gödel gives 0.2), then compute
`∂/∂a` for each at a=0.9,b=0.2 and confirm the product gradient (0.2) is nonzero while the Gödel
gradient (0) is zero. Then modify the PyTorch loop to use the Gödel t-norm-based implication
(`godel_tconorm(soft_not(p), q)`) instead of the Łukasiewicz implication, and observe empirically
that training is slower or stalls for some instances — connect this directly to the zero-gradient
argument in §3.
