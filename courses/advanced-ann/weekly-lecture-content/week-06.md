# Week 6 — Lecture Content: Sharpness and Generalization

## 1. The Flat-vs-Sharp-Minima Intuition
Two parameter settings $w_1, w_2$ can both achieve near-zero training loss while behaving very
differently under small perturbations: at a **flat** minimum, $L(w_1+\epsilon)$ rises slowly as
$\epsilon$ grows; at a **sharp** minimum, $L(w_2+\epsilon)$ rises quickly. The generalization
argument is: the training loss surface is itself a noisy estimate of the true population loss
surface (different finite samples give slightly different surfaces), so a minimum that is robust
to small perturbations of its *location* should also be more robust to the *mismatch* between the
training surface and the population surface it approximates — predicting lower test loss.

## 2. Measuring Sharpness
The most common operational proxy is the **top eigenvalue of the Hessian**, $\lambda_{\max}(\nabla^2
L(w))$, at a found minimum — large $\lambda_{\max}$ means loss rises quickly along at least one
direction. Computing it exactly requires forming the (prohibitively large, per the graduate
course's optimization-theory week) Hessian; in practice it is estimated via **power iteration
using Hessian-vector products** (computable via two backward passes, without ever forming the full
Hessian): starting from a random unit vector $v_0$, repeat
$$
v_{k+1} = \frac{\nabla^2 L(w)\, v_k}{\|\nabla^2 L(w)\, v_k\|}, \qquad
\lambda_{\max} \approx v_k^\top \nabla^2 L(w)\, v_k .
$$
A cheaper, widely used alternative is a **perturbation-based proxy**: the increase in loss under
an adversarially chosen small perturbation of bounded norm, $\max_{\|\epsilon\|\le\rho} L(w+\epsilon)
- L(w)$, which avoids an explicit eigenvalue computation and is exactly the quantity SAM (§4)
is built to directly control.

## 3. The Reliability Critique
The naive "flat minima generalize better" story has a well-known problem: standard sharpness
measures are **not invariant to function-preserving reparameterizations** of a ReLU network. For
example, in a two-layer ReLU network, scaling one layer's incoming weights by $\alpha>0$ and the
next layer's outgoing weights by $1/\alpha$ computes the *exact same function* (ReLU is positively
homogeneous: $\mathrm{ReLU}(\alpha x) = \alpha\,\mathrm{ReLU}(x)$ for $\alpha>0$), yet changes the
local curvature of the loss with respect to the parameters — so $\lambda_{\max}$ measured at the
two reparameterizations of the identical function can differ substantially. This means raw
sharpness, as conventionally measured, is partly an artifact of an arbitrary parameterization
choice rather than a property of the function the network computes — a serious objection to
treating sharpness as a clean, causal explanation of generalization, rather than merely a
correlated, measurement-dependent observation. Proposed fixes (e.g., normalizing sharpness by
weight norm, or defining scale-invariant sharpness measures) partially address this but do not
fully settle the debate; this course presents the sharpness-generalization relationship as a real,
reproducible empirical correlation whose causal and measurement-theoretic status remains actively
debated, not as a closed theoretical account.

## 4. Sharpness-Aware Minimization (SAM), Derived
SAM responds to the flat-minima intuition directly by training to minimize *worst-case* loss in a
neighborhood, rather than hoping flatness emerges as a side effect of plain SGD. The objective:
$$
\min_w \; \max_{\|\epsilon\|_2 \le \rho} L(w+\epsilon).
$$
This inner maximization is itself intractable to solve exactly at every step, so SAM uses a
**first-order approximation**: linearize $L(w+\epsilon) \approx L(w) + \epsilon^\top \nabla L(w)$,
whose maximizer over $\|\epsilon\|_2\le\rho$ is simply the gradient direction scaled to norm $\rho$:
$$
\hat\epsilon(w) = \rho\, \frac{\nabla L(w)}{\|\nabla L(w)\|_2}.
$$
SAM's practical update is then a **two-step** procedure per iteration:
1. Compute $\hat\epsilon(w_t)$ from the gradient at the current point, and form the perturbed point
   $w_t + \hat\epsilon(w_t)$.
2. Compute the gradient *at the perturbed point*, $\nabla L(w_t+\hat\epsilon(w_t))$, and use **that**
   gradient (not $\nabla L(w_t)$) for the actual parameter update:
   $$
   w_{t+1} = w_t - \eta\, \nabla L\big(w_t+\hat\epsilon(w_t)\big).
   $$
This costs exactly one extra forward-backward pass per step compared to plain SGD, and empirically
finds minima with measurably lower sharpness (by the §2 proxies) and, on many benchmarks, better
test accuracy — a concrete, practically useful method whose empirical success is one of the
strongest pieces of evidence for *some* real sharpness-generalization connection, even though §3's
reparameterization critique means the full causal story remains open.

## 5. Code: Hessian Top Eigenvalue via Power Iteration, and a Minimal SAM Step
```python
import torch
import torch.nn as nn

torch.manual_seed(0)

def hvp(loss, params, v):
    grads = torch.autograd.grad(loss, params, create_graph=True)
    flat_grad = torch.cat([g.flatten() for g in grads])
    dot = (flat_grad * v).sum()
    hv = torch.autograd.grad(dot, params, retain_graph=True)
    return torch.cat([h.flatten() for h in hv])

def top_hessian_eigenvalue(model, loss_fn, n_iter=30):
    params = list(model.parameters())
    n_params = sum(p.numel() for p in params)
    v = torch.randn(n_params)
    v = v / v.norm()
    loss = loss_fn()
    for _ in range(n_iter):
        Hv = hvp(loss, params, v)
        v = Hv / (Hv.norm() + 1e-12)
    Hv = hvp(loss, params, v)
    return (v @ Hv).item()

def sam_step(model, loss_fn, opt, rho=0.05):
    loss = loss_fn()
    grads = torch.autograd.grad(loss, model.parameters())
    flat = torch.cat([g.flatten() for g in grads])
    eps_scale = rho / (flat.norm() + 1e-12)
    originals = [p.detach().clone() for p in model.parameters()]
    with torch.no_grad():
        for p, g in zip(model.parameters(), grads):
            p.add_(eps_scale * g)
    opt.zero_grad()
    perturbed_loss = loss_fn()
    perturbed_loss.backward()
    with torch.no_grad():
        for p, orig in zip(model.parameters(), originals):
            p.copy_(orig)
    opt.step()
    return loss.item()

model = nn.Sequential(nn.Linear(10, 32), nn.ReLU(), nn.Linear(32, 1))
X, y = torch.randn(64, 10), torch.randn(64, 1)
loss_fn = lambda: ((model(X) - y) ** 2).mean()
opt = torch.optim.SGD(model.parameters(), lr=0.05)

for step in range(200):
    train_loss = sam_step(model, loss_fn, opt, rho=0.05)

sharpness = top_hessian_eigenvalue(model, loss_fn)
print(f"Final training loss: {train_loss:.4f}  Estimated top Hessian eigenvalue: {sharpness:.4f}")
```
Students compare `top_hessian_eigenvalue` after SAM training against the same architecture trained
with plain SGD for the same number of steps on the same data, and should observe a measurably
lower sharpness value for the SAM-trained model.

## 6. In-Class Exercise
A colleague claims: "SAM's empirical success definitively proves that flat minima cause better
generalization." Using §3's reparameterization critique, state a weaker, more defensible
conclusion that SAM's results actually support.
