# Week 12 — Lecture Content: The Neural Tangent Kernel

## 1. Definition
For a network $f(x;\theta)$ with parameters $\theta\in\mathbb{R}^p$, the **Neural Tangent Kernel
(NTK)** between two inputs $x, x'$ is
$$
\Theta(x,x';\theta) = \nabla_\theta f(x;\theta)^\top \nabla_\theta f(x';\theta) = \sum_{k=1}^p \frac{\partial f(x;\theta)}{\partial\theta_k}\,\frac{\partial f(x';\theta)}{\partial\theta_k}
$$
— the inner product of the two inputs' parameter-gradients. It measures how similarly a small
change in $\theta$ affects the network's output at $x$ versus at $x'$: if $\Theta(x,x')$ is large,
nudging $\theta$ to improve the fit at $x$ tends to also move the output at $x'$ in a correlated
way.

## 2. Jacot, Gabriel, and Hongler's Central Result
Jacot, Gabriel, and Hongler's Neural Tangent Kernel framework establishes that, under a specific
("NTK") parameterization and in the **infinite-width limit**, two remarkable things happen: (1)
$\Theta(x,x';\theta_0)$ at initialization converges to a **deterministic** limiting kernel
$\Theta^\infty(x,x')$ (the randomness of the specific initialization washes out as width grows),
and (2) this kernel stays **(approximately) constant throughout gradient-descent training** —
the network's parameters move, but the *kernel* they induce barely does. Under these two facts,
the network's output function evolves during training exactly as **kernel regression** against
the fixed kernel $\Theta^\infty$ would: a provably tractable, (in a precise sense) *convex-like*
optimization problem in function space, even though the original optimization problem over
$\theta$ is highly non-convex.

## 3. What This Reveals, and Its Limits
**What it explains:** a theoretically grounded account of why sufficiently wide networks are easy
to drive to (near-)zero training loss via plain gradient descent — in the infinite-width limit,
training is not navigating a complicated non-convex landscape in function space at all, but
performing kernel regression, which has no bad local minima.

**Its limits:** the NTK is **fixed** — by construction, the infinite-width argument shows the
kernel does not change during training, which means this regime provably involves **no feature
learning** (the network's internal representations do not adapt to the data; only the final
linear combination implied by kernel regression does). This is directly at odds with the widely
held view that feature learning is central to why realistic, *finite*-width networks generalize as
well as they do in practice. So NTK theory is a genuinely useful, rigorous account of
**trainability** in a specific limiting regime, not a complete theory of why trained networks
**generalize** well — a distinction this course keeps explicit rather than blurring the two
questions.

## 4. Code: A Small-Scale NTK Computation
```python
import torch, torch.nn as nn

torch.manual_seed(0)

class OneHiddenLayer(nn.Module):
    def __init__(self, in_dim, width):
        super().__init__()
        self.w1 = nn.Parameter(torch.randn(width, in_dim) / in_dim**0.5)
        self.v = nn.Parameter(torch.randn(width) / width**0.5)

    def forward(self, x):
        return (torch.relu(x @ self.w1.T) @ self.v)

def ntk_entry(model, x1, x2):
    params = list(model.parameters())
    g1 = torch.autograd.grad(model(x1), params, create_graph=False, retain_graph=True)
    model.zero_grad()
    g2 = torch.autograd.grad(model(x2), params, create_graph=False, retain_graph=True)
    return sum((a.flatten() @ b.flatten()).item() for a, b in zip(g1, g2))

width, in_dim = 2000, 3
model = OneHiddenLayer(in_dim, width)
X = torch.randn(6, in_dim)

K = torch.zeros(6, 6)
for i in range(6):
    for j in range(6):
        K[i, j] = ntk_entry(model, X[i:i+1], X[j:j+1])
print("Empirical NTK (6x6) at initialization:\n", K)

# Kernel-regression prediction using this empirical NTK vs. actually training the network.
y = torch.sign(X[:, 0] + X[:, 1])                       # a simple target
alpha = torch.linalg.solve(K + 1e-3 * torch.eye(6), y)   # kernel ridge regression coefficients

opt = torch.optim.SGD(model.parameters(), lr=0.5)
for _ in range(500):
    opt.zero_grad()
    loss = ((model(X).squeeze() - y) ** 2).mean()
    loss.backward(); opt.step()
trained_pred = model(X).squeeze().detach()
print("Trained-network predictions:", trained_pred)
print("Target y:                  ", y)
```
With a large enough width, the trained network's predictions on the training points should land
close to the targets $y$ (near-perfect fit), consistent with the NTK account of easy trainability
at large width; students are encouraged to compare predictions *off* the training set from the
trained network against kernel-regression predictions using $K$ evaluated at new points, to see
the two begin to agree as width grows and increasingly diverge as width shrinks (since the fixed-
kernel approximation degrades away from the infinite-width limit).

## 5. In-Class Exercise
Explain, in one or two sentences, why "the NTK stays approximately constant during training" is
logically equivalent to "the network is not learning new features" in the infinite-width limit,
and why this is a limitation rather than a strength of the theory as an account of generalization.
