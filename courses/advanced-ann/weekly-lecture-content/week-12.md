# Week 12 — Lecture Content: Loss-Landscape Geometry and the Lottery Ticket Hypothesis Revisited

## 1. Mode Connectivity
Train two networks of identical architecture from two different random initializations (and/or
with two different random data orderings) to two different minima $\theta_A$, $\theta_B$, both
achieving low training loss. The **linear interpolation**
$$
\theta(\lambda) = (1-\lambda)\,\theta_A + \lambda\,\theta_B, \qquad \lambda\in[0,1],
$$
typically shows loss $L(\theta(\lambda))$ **rising substantially** for intermediate $\lambda$ — a
"loss barrier" between the two minima — the classical evidence that independently trained minima
sit in separate basins. **Mode connectivity** is the empirical finding that, despite this linear
barrier, a simple **nonlinear path** between $\theta_A$ and $\theta_B$ — for example, a
piecewise-linear path through one or two learned intermediate "bend points" $\theta_1,\dots$, or a
quadratic Bezier curve $\theta(\lambda) = (1-\lambda)^2\theta_A + 2\lambda(1-\lambda)\theta_1 +
\lambda^2\theta_B$ with $\theta_1$ optimized to minimize the loss integrated along the curve — can
connect the same two minima while keeping loss **low along the entire path**.

**What this does and does not imply.** It implies that the two minima are not isolated,
disconnected basins in the strong sense a high linear barrier might suggest — there *is* a
low-loss route between them, geometrically — suggesting the landscape's many apparent minima may
be better described as points on (or near) a single, connected, low-loss manifold that a naive
straight-line interpolation simply fails to stay on. It does **not** imply the two minima compute
the same function, that all minima are connected to each other (connectivity is typically checked
pairwise, not established as transitive across arbitrarily many minima at once), or that the
landscape is "flat" in any sharpness sense (Week 6) — a connected low-loss manifold can still have
substantial curvature transverse to the connecting path.

## 2. The Lottery Ticket Hypothesis, Recapped (One Paragraph, Not Re-Derived)
The graduate course established Frankle and Carbin's claim: a randomly-initialized dense network
contains a sparse subnetwork ("winning ticket") that, trained in isolation **from that same
initialization**, matches the full network's accuracy, found via iterative magnitude pruning; a
randomly re-initialized copy of the same sparse mask typically fails to train as well, which was
read as evidence that the specific initialization (not just the sparse topology) matters.

## 3. Current Refinements: Linear Mode Connectivity
A significant refinement connects the Lottery Ticket Hypothesis directly to §1's mode-connectivity
material: a found winning ticket's training trajectory, when retrained (e.g., with a different
data order or different training noise) from its original initialization, tends to stay **linearly
mode-connected** to its first training run's final solution — i.e., the straight-line interpolation
between the two independent retrainings of the *same* winning-ticket initialization shows **no**
loss barrier, unlike the generic case in §1. A randomly reinitialized copy of the same sparse mask
typically does *not* show this linear connectivity between its own independent retrainings. This
refinement gives a sharper, more geometrically precise characterization of what "this initialization
matters" means than the original accuracy-matching criterion alone: it is not merely that training
from the original initialization reaches a comparably *accurate* solution, but that it reliably
reaches a solution in the *same linearly-connected region* of the landscape, which is a stronger and
independently checkable claim.

## 4. Current Critiques
- **Scale and learning-rate sensitivity.** At the learning rates and network scales used for the
  largest modern networks, iterative magnitude pruning has been found to require specific
  adjustments (e.g., "rewinding" to an early-training checkpoint rather than the literal random
  initialization) to reliably find winning tickets at all — complicating the original claim's
  literal scope and suggesting the clean initialization story may need qualification at scale.
- **What the pruning procedure's evidence actually establishes.** A standing open question: is the
  evidence from iterative magnitude pruning best read as "a sparse, well-performing subnetwork was
  already implicitly present, favorably positioned, at the original initialization" (the strong
  reading the original hypothesis suggests), or is it better read as evidence mainly about the
  *iterative pruning procedure itself* — which uses information from a full training run (to decide
  what to prune) that is not available to the sparse network "from the start" — rather than a
  claim about initialization alone? Current literature has not fully settled which reading is more
  accurate, and this course presents both as live possibilities rather than asserting either as
  the settled interpretation.

## 5. Code: Linear vs. Nonlinear (Bend-Point) Interpolation Between Minima
```python
import torch
import torch.nn as nn

torch.manual_seed(0)

def make_model():
    return nn.Sequential(nn.Linear(10, 32), nn.ReLU(), nn.Linear(32, 1))

def train(model, X, y, steps=1500, lr=0.05):
    opt = torch.optim.SGD(model.parameters(), lr=lr)
    for _ in range(steps):
        opt.zero_grad()
        loss = ((model(X) - y) ** 2).mean()
        loss.backward()
        opt.step()
    return model

def flatten(model):
    return torch.cat([p.detach().flatten() for p in model.parameters()])

def set_params(model, flat):
    i = 0
    for p in model.parameters():
        n = p.numel()
        p.data.copy_(flat[i:i+n].view(p.shape))
        i += n

def loss_at(model, flat, X, y):
    set_params(model, flat)
    with torch.no_grad():
        return ((model(X) - y) ** 2).mean().item()

X, y = torch.randn(100, 10), torch.randn(100, 1)

model_A = train(make_model(), X, y)
model_B = train(make_model(), X, y)   # different random init -> different minimum
theta_A, theta_B = flatten(model_A), flatten(model_B)
probe = make_model()

print("Linear interpolation:")
for lam in [0.0, 0.25, 0.5, 0.75, 1.0]:
    theta_lam = (1 - lam) * theta_A + lam * theta_B
    print(f"  lambda={lam:.2f}  loss={loss_at(probe, theta_lam, X, y):.4f}")

# Optimize a single bend point theta_1 for a quadratic Bezier path.
theta_1 = ((theta_A + theta_B) / 2).clone().requires_grad_(True)
opt = torch.optim.Adam([theta_1], lr=0.01)
for _ in range(300):
    opt.zero_grad()
    total = 0.0
    for lam in torch.linspace(0.0, 1.0, 5):
        theta_lam = (1 - lam) ** 2 * theta_A + 2 * lam * (1 - lam) * theta_1 + lam ** 2 * theta_B
        set_params(probe, theta_lam)
        total = total + ((probe(X) - y) ** 2).mean()
    total.backward()
    opt.step()

print("Nonlinear (Bezier, optimized bend point) path:")
for lam in [0.0, 0.25, 0.5, 0.75, 1.0]:
    theta_lam = (1 - lam) ** 2 * theta_A + 2 * lam * (1 - lam) * theta_1.detach() + lam ** 2 * theta_B
    print(f"  lambda={lam:.2f}  loss={loss_at(probe, theta_lam, X, y):.4f}")
```
The linear path should show a clear loss bump at intermediate $\lambda$; the optimized Bezier path
should show a substantially lower loss across all intermediate $\lambda$, demonstrating mode
connectivity directly.

## 6. In-Class Exercise
Explain, using §3's linear-mode-connectivity refinement, what specific additional experiment
(beyond re-measuring final accuracy) a student should run to check whether a sparse mask they
found is a genuine "winning ticket" in the stronger, geometric sense, versus merely an
accuracy-matching sparse subnetwork.
