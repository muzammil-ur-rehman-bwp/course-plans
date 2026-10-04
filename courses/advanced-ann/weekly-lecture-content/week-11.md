# Week 11 — Lecture Content: Modern Generalization Bounds — PAC-Bayes

## 1. Why Classical Bounds Are Often Vacuous for Deep Networks
The graduate course's classical generalization bounds (VC dimension, Rademacher complexity) bound
the *worst-case* gap between training and test error **uniformly over an entire hypothesis
class** — every function the class can represent is treated as equally likely to be the one
training actually finds. For a deep network, the relevant capacity measure (VC dimension scales at
least linearly, and in the worst case much faster, with parameter count) is enormous, and the
resulting bound, evaluated numerically for realistic parameter counts and dataset sizes, routinely
exceeds $1$ — i.e., it is weaker than the trivial bound "generalization gap $\le 1$" for a
loss bounded in $[0,1]$, making it **vacuous**: true, but uninformative. This is not a minor
numerical inconvenience; it means the classical framework, applied literally, provides *no*
genuine evidence that deep networks should generalize at all — the strong *empirical*
generalization observed must be explained by something the worst-case, class-uniform bound does
not capture.

## 2. The PAC-Bayes Idea
PAC-Bayes bounds change the question: instead of bounding worst-case error uniformly over an
entire hypothesis class, they bound the **expected** error of a hypothesis drawn from a specific
**posterior distribution** $Q$ over the hypothesis class — the distribution a specific learning
algorithm, run on this specific training set, actually produces — compared against a fixed,
**data-independent prior** $P$ chosen *before* seeing the data. The core trade-off: the bound's
tightness depends not on the raw size of the whole hypothesis class, but on how far the learned
posterior $Q$ has moved from the prior $P$, measured by the Kullback-Leibler divergence
$\mathrm{KL}(Q\|P)$. A commonly used form of the bound states that, with high probability
(at least $1-\delta$) over the draw of the $n$-sample training set, **simultaneously for every**
posterior $Q$:
$$
\mathbb E_{h\sim Q}\big[R(h)\big] \;\le\; \mathbb E_{h\sim Q}\big[\hat R(h)\big] \;+\;
\sqrt{\frac{\mathrm{KL}(Q\|P) + \ln(c\sqrt{n}/\delta)}{2n}},
$$
where $R(h)$ is true (population) risk, $\hat R(h)$ is empirical (training) risk, and $c$ is a
small constant that varies slightly by the specific version of the bound in the literature (the
structural form — an empirical-risk term plus a square-root term combining a KL-divergence
penalty and a confidence term, both divided by sample size inside the square root — is what
matters here, not memorizing one specific constant). Because this bound holds **simultaneously
for every $Q$** (a "uniform-over-posteriors" statement proven once, not re-proven per $Q$), one is
free to choose or construct $Q$ to make the bound as tight as possible for a specific trained
network, which is exactly where sharpness (Week 6) becomes relevant.

## 3. Why PAC-Bayes Can Be Non-Vacuous Where VC/Rademacher Is Not
Crucially, $\mathrm{KL}(Q\|P)$ does **not** scale directly with raw parameter count the way
VC dimension does — it scales with how much the specific learned posterior actually differs from
the prior, which can be made small even for a network with very many parameters, *provided* a
good choice of prior and posterior is available. The natural construction: let $Q$ be a Gaussian
distribution centered at the trained weights $w^\star$, with some covariance (often a scaled
identity), and let $P$ be a Gaussian prior centered at (e.g.) the initialization, with a similar
form. Then
$$
\mathrm{KL}(Q\|P) = \frac12\left(\frac{\|w^\star - w_0\|^2}{\sigma_P^2} + d\left(\frac{\sigma_Q^2}{\sigma_P^2} - 1 - \ln\frac{\sigma_Q^2}{\sigma_P^2}\right)\right)
$$
for isotropic Gaussians of dimension $d$ with posterior variance $\sigma_Q^2$ and prior variance
$\sigma_P^2$ — and this quantity is controlled not by $d$ alone but by how large a *posterior
variance* $\sigma_Q^2$ one can tolerate while keeping $\mathbb E_{h\sim Q}[\hat R(h)]$ (training
risk *averaged over the posterior's random perturbations*, not just at the single point $w^\star$)
still small. **This is precisely Week 6's sharpness condition**: a network at a *flat* minimum
tolerates a large $\sigma_Q^2$ (large random perturbations barely increase training risk) while
keeping the first term in the bound small, directly tightening the bound; a network at a *sharp*
minimum forces a small $\sigma_Q^2$ to keep $\mathbb E_{h\sim Q}[\hat R(h)]$ controlled, which
inflates $\mathrm{KL}(Q\|P)$ and loosens the bound. PAC-Bayes is, in this precise sense, the
generalization-theoretic framework that gives sharpness a rigorous, quantitative role in an actual
provable bound, rather than only an empirically observed correlation.

## 4. Honest Limits
PAC-Bayes bounds computed this way can be numerically non-vacuous for realistic deep networks —
a genuine advance over classical bounds — but they are not a complete, assumption-free theory
either: the bound's tightness depends heavily on the choice of prior (an unlucky or poorly matched
prior can still produce a loose bound even for a good posterior), and the Gaussian-perturbation
posterior construction above is a convenient, tractable choice, not a unique or obviously optimal
one. This course presents PAC-Bayes as the current best-established tighter alternative to
classical bounds, not as a fully settled, final theory of deep-network generalization.

## 5. Code: A Simple PAC-Bayes Bound vs. a Classical Bound
```python
import torch
import torch.nn as nn
import math

torch.manual_seed(0)

d_in, width = 8, 64
model = nn.Sequential(nn.Linear(d_in, width), nn.ReLU(), nn.Linear(width, 1))
n = 200
X = torch.randn(n, d_in)
y = torch.sign(X[:, 0] + 0.5 * X[:, 1]).unsqueeze(1).float()

opt = torch.optim.Adam(model.parameters(), lr=1e-2)
for _ in range(500):
    opt.zero_grad()
    pred = torch.tanh(model(X))
    loss = ((pred - y) ** 2).mean()
    loss.backward()
    opt.step()

w_star = torch.cat([p.detach().flatten() for p in model.parameters()])
d = w_star.numel()
w0 = torch.zeros_like(w_star)   # prior centered at a generic (e.g., zero) reference point
sigma_p2, sigma_q2 = 0.05, 0.01

def empirical_risk_under_posterior(model, X, y, sigma_q, n_samples=20):
    params = list(model.parameters())
    shapes = [p.shape for p in params]
    risks = []
    originals = [p.detach().clone() for p in params]
    for _ in range(n_samples):
        with torch.no_grad():
            for p, orig in zip(params, originals):
                p.copy_(orig + sigma_q * torch.randn_like(p))
            pred = torch.tanh(model(X))
            risks.append(((pred - y) ** 2).mean().item())
    with torch.no_grad():
        for p, orig in zip(params, originals):
            p.copy_(orig)
    return sum(risks) / len(risks)

emp_risk_q = empirical_risk_under_posterior(model, X, y, math.sqrt(sigma_q2))
kl = 0.5 * (((w_star - w0) ** 2).sum() / sigma_p2
            + d * (sigma_q2 / sigma_p2 - 1 - math.log(sigma_q2 / sigma_p2)))
delta = 0.05
pac_bayes_bound = emp_risk_q + math.sqrt((kl.item() + math.log(2 * math.sqrt(n) / delta)) / (2 * n))

vc_dim_proxy = d  # a crude, deliberately generous VC-dimension proxy for a network with d parameters
classical_bound = math.sqrt(vc_dim_proxy * math.log(2 * math.e * n / vc_dim_proxy) / n)  # VC-style form

print(f"Posterior-averaged empirical risk: {emp_risk_q:.4f}")
print(f"PAC-Bayes bound on true risk:       {pac_bayes_bound:.4f}")
print(f"Classical VC-style bound (proxy):   {classical_bound:.4f}")
```
With a reasonably trained network and a modest `sigma_q2`, the PAC-Bayes bound typically comes out
well below $1$ (non-vacuous), while the crude VC-style proxy — using the network's full parameter
count as a stand-in capacity measure — typically exceeds $1$ for any width/sample-size combination
realistic for a deep network, illustrating §1's vacuity claim directly.

## 6. In-Class Exercise
Using §3's KL formula, explain why increasing `sigma_q2` (a flatter, more spread-out posterior)
has two opposing effects on the PAC-Bayes bound, and under what condition on the model's sharpness
the net effect is a tighter bound rather than a looser one.
