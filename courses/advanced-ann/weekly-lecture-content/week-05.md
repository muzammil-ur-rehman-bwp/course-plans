# Week 5 — Lecture Content: Feature Learning Beyond the Kernel Regime

## 1. The Question This Week Answers
Weeks 2–3 established two valid infinite-width idealizations of a wide network: the NTK limit
(frozen kernel, no feature learning) and the mean-field limit (a distribution that can move,
admitting feature learning). Neither limit *is* a practical, finite-width network. This week asks
directly: where do practically-sized networks actually sit, and what escapes the lazy regime as
width shrinks from infinity toward practice?

## 2. Mechanisms That Break the Lazy-Training Approximation
Recall from Week 2 that lazy training requires relative parameter movement
$\|\theta_t-\theta_0\|/\|\theta_0\|\to 0$. Three concrete factors push a finite-width network away
from this condition, each directly reversing one piece of the Week 2 argument:
1. **Finite width.** The self-averaging argument behind $\Theta_0\to\Theta^\infty$ and the
   Hessian-shrinking argument behind $\Theta_t\approx\Theta_0$ are both asymptotic in width; at
   finite width both have $O(1/\sqrt{\text{width}})$-type residual fluctuations that do not vanish.
2. **Output-scale and parameterization choices.** The NTK parameterization's $1/\sqrt{m}$ output
   scaling is itself a *choice*; a "standard" parameterization (no such compensating scale factor,
   as is typically used in practice) does not enforce small relative movement, and is the
   parameterization under which practitioners observe the strongest feature-learning effects.
3. **Learning rate and training horizon.** A sufficiently large learning rate or long enough
   training time can push parameters an $O(1)$ relative distance from initialization even at
   width where the asymptotic argument would otherwise predict near-laziness at smaller learning
   rates — the lazy-vs-feature-learning regime is not purely a function of width, but of width
   together with the effective training dynamics applied to it.

## 3. Empirical Signatures of Feature Learning
Three measurable quantities distinguish a network's training run as (closer to) lazy/kernel-regime
or (closer to) feature-learning regime, each directly testable in code:
- **Kernel drift.** $\|\Theta_T - \Theta_0\|/\|\Theta_0\|$ over the course of training (exactly
  the quantity measured in Week 2's lab). Large, sustained drift indicates the kernel — and hence
  the implicit featurization it encodes — is not fixed; the network's effective "features" are
  changing.
- **Hidden-representation task-alignment.** Whether a hidden layer's learned representation
  becomes more linearly predictive of the task label over training than it was at initialization
  (e.g., via a linear probe trained on frozen hidden activations at different training checkpoints)
  — a direct signature that the representation itself, not just the final readout, has adapted.
- **The kernel-regression-vs-trained-network performance gap.** Comparing (a) predictions from
  kernel ridge regression using the *empirical* NTK measured at initialization against (b)
  predictions from actually training the network with gradient descent, on the same data. Under
  the Week 2 theory this gap should shrink as width grows (approaching the NTK limit); if, at a
  *practically used* width, (b) outperforms (a) by a wide and non-shrinking margin, that gap is
  itself evidence that something beyond fixed-kernel regression — i.e., feature learning — is
  doing real work at that width.

## 4. Why This Matters for Explaining Deep Learning's Success
The widely held view in current theoretical ML research is that feature learning — not a fixed,
data-independent random-feature kernel — is central to why realistic, finite-width networks
generalize as well as they empirically do, particularly on structured, high-dimensional data
(images, text) where a hand-designed or random fixed feature map is known to perform far worse
than a trained, adapted representation. The NTK limit, by construction (Week 2, §2), cannot
address this question at all — it is not a limitation that more careful NTK analysis can resolve,
because the limit's entire content is that the kernel does *not* adapt. This is why feature
learning is treated in current research as a necessary, separate theoretical object from NTK
theory, not a refinement of it: NTK theory is a rigorous, useful account of *trainability* in a
specific limiting regime, and this week's material is the current, less-settled account of
*adaptation*, which many researchers hold to be the more important of the two questions for
explaining practical generalization.

## 5. Code: The Central Width Comparison
```python
import torch
import torch.nn as nn

torch.manual_seed(0)

class MLP(nn.Module):
    def __init__(self, in_dim, width, ntk_param=True):
        super().__init__()
        self.w1 = nn.Parameter(torch.randn(width, in_dim) / in_dim**0.5)
        self.b1 = nn.Parameter(torch.zeros(width))
        scale = width**0.5 if ntk_param else width
        self.w2 = nn.Parameter(torch.randn(width) / scale)

    def forward(self, x):
        return torch.relu(x @ self.w1.T + self.b1) @ self.w2

def ntk_matrix(model, X):
    params = list(model.parameters())
    grads = []
    for i in range(X.shape[0]):
        model.zero_grad()
        out = model(X[i:i+1])
        g = torch.autograd.grad(out, params, retain_graph=False)
        grads.append(torch.cat([gi.flatten() for gi in g]))
    G = torch.stack(grads)
    return G @ G.T

def kernel_regression_predict(K_train, y_train, K_test_train, ridge=1e-3):
    alpha = torch.linalg.solve(K_train + ridge * torch.eye(len(y_train)), y_train)
    return K_test_train @ alpha

def experiment(width, n_train=12, n_test=8, steps=3000, lr=0.2):
    model = MLP(in_dim=4, width=width)
    X_train = torch.randn(n_train, 4)
    X_test = torch.randn(n_test, 4)
    y_train = torch.sin(X_train[:, 0] * 2) + X_train[:, 1] * X_train[:, 2]
    y_test = torch.sin(X_test[:, 0] * 2) + X_test[:, 1] * X_test[:, 2]

    K0 = ntk_matrix(model, torch.cat([X_train, X_test]))
    K0_train = K0[:n_train, :n_train]
    K0_test_train = K0[n_train:, :n_train]
    kernel_pred = kernel_regression_predict(K0_train, y_train, K0_test_train)
    kernel_mse = ((kernel_pred - y_test) ** 2).mean().item()

    opt = torch.optim.SGD(model.parameters(), lr=lr)
    for _ in range(steps):
        opt.zero_grad()
        loss = 0.5 * ((model(X_train) - y_train) ** 2).sum()
        loss.backward()
        opt.step()
    trained_mse = ((model(X_test) - y_test) ** 2).mean().item()

    K_T = ntk_matrix(model, torch.cat([X_train, X_test]))
    kernel_drift = (K_T - K0).norm().item() / K0.norm().item()
    return kernel_mse, trained_mse, kernel_drift

for width in [20, 100, 1000]:
    k_mse, t_mse, drift = experiment(width)
    print(f"width={width:5d}  kernel-regression test MSE={k_mse:.4f}  "
          f"trained-network test MSE={t_mse:.4f}  kernel drift={drift:.4f}")
```
Expect `kernel drift` to shrink with width (as Week 2 predicts), and the gap between
`kernel-regression test MSE` and `trained-network test MSE` to be largest at the smallest widths
tried here — the regime where feature learning is doing the most additional work relative to the
fixed-kernel baseline.

## 6. In-Class Exercise
Using the three signatures in §3, design (in words, no need to run it) an experiment that would
distinguish "this network is in the lazy/kernel regime" from "this network is feature-learning,"
for a width your instructor assigns, and state what result on each of the three signatures would
count as evidence for each hypothesis.
