# Week 3 — Lecture Content: Advanced Diffusion Techniques

## 1. Conditional Generation and Classifier Guidance
To generate samples conditioned on $c$ (a class label, a text prompt, etc.), we want the reverse
SDE's score replaced by the **conditional score** $\nabla_x\log p_t(x\mid c)$. One early approach
(classifier guidance) trains a separate classifier $p_t(c\mid x)$ on noisy $x_t$ and uses Bayes'
rule in gradient form:
```
∇ₓ log p_t(x | c) = ∇ₓ log p_t(x) + ∇ₓ log p_t(c | x)
```
(from $p_t(x\mid c) \propto p_t(x)\,p_t(c\mid x)$, take $\log$ and differentiate w.r.t. $x$; the
normalizing constant $p_t(c)$ does not depend on $x$ and drops out). This requires training and
maintaining an extra classifier on noisy inputs at every diffusion step — exactly the dependency
**classifier-free guidance** removes.

## 2. Classifier-Free Guidance, Derived
Train a single score network $s_\theta(x,t,c)$ that also accepts an unconditional input by
randomly replacing $c$ with a null token $\varnothing$ during training (e.g., 10–20% of the time),
so the same network represents both $s_\theta(x,t,c) \approx \nabla_x\log p_t(x\mid c)$ and
$s_\theta(x,t,\varnothing) \approx \nabla_x\log p_t(x)$. From Section 1's identity,
$\nabla_x\log p_t(c\mid x) = \nabla_x\log p_t(x\mid c) - \nabla_x\log p_t(x) \approx
s_\theta(x,t,c) - s_\theta(x,t,\varnothing)$ — **the implicit classifier's score, obtained with no
classifier at all**, just a difference of two score-network outputs. Substituting this estimate
back into a *generalized* classifier-guidance formula with a guidance weight $w$,
$\tilde s = \nabla_x\log p_t(x) + w\,\nabla_x\log p_t(c\mid x)$ — note $w=1$ recovers plain
classifier guidance, $w=0$ recovers the unconditional score — and using the two network outputs in
place of each term:
```
s̃_θ(x, t, c) = s_θ(x, t, ∅) + w · ( s_θ(x, t, c) − s_θ(x, t, ∅) )
             = (1 − w) s_θ(x, t, ∅) + w · s_θ(x, t, c)
```
This is the **classifier-free guidance** score. Substituting $\tilde s$ for $s_\theta$ in the
reverse-SDE sampler of Week 2, $w\geq 1$, is exactly the standard formula. Re-deriving where $w$
enters: the guided score corresponds to sampling (approximately) from a distribution proportional
to $p_t(x\mid c)\, p_t(c\mid x)^{w-1}$ — raising the implicit classifier's confidence to the power
$w-1$ sharpens the distribution toward high-confidence regions for $c$ as $w$ grows past 1.

```python
import torch

def classifier_free_guidance(score_cond, score_uncond, w):
    """score_cond, score_uncond: tensors of shape (batch, dim), same network evaluated at
    (x,t,c) and (x,t,∅) respectively. w=1 -> plain conditional; w=0 -> unconditional."""
    return (1 - w) * score_uncond + w * score_cond
```

## 3. The Diversity/Fidelity Tradeoff
At $w=1$, sampling follows the plain conditional score. As $w$ grows past 1, the effective target
distribution $p_t(x\mid c)\,p_t(c\mid x)^{w-1}$ concentrates probability mass on $x$ for which the
implicit classifier is most confident $c$ is the correct label — this typically produces samples
that look more canonically "like $c$" (higher condition-adherence/fidelity) at the cost of sample
diversity (the distribution's effective support shrinks). There is no free lunch: $w$ is a dial
trading one for the other, not a quality knob that is always better turned up.

## 4. Flow Matching: A Different Continuous-Time Framework
The graduate course's normalizing flows are trained by exact maximum likelihood, which requires
computing $\log\det$ of the transformation's Jacobian at every layer — expensive and architecturally
restrictive (invertibility, tractable Jacobian). **Flow matching** sidesteps this entirely.
Instead of specifying a noising SDE and matching its score, flow matching directly specifies a
(simple, often linear) **probability path** interpolating between a noise sample $x_0\sim\mathcal
N(0,I)$ and a data sample $x_1 \sim p_{\mathrm{data}}$:
```
x_t = (1 - t) x_0 + t x_1,        t ∈ [0, 1]
```
This path has a known, closed-form **target velocity** at every point along it:
$u_t(x_t \mid x_0, x_1) = dx_t/dt = x_1 - x_0$ (constant along a linear path). A neural velocity
field $v_\theta(x,t)$ is trained by simple regression onto this target — no simulation, no
likelihood, no Jacobian:
```
L_FM(θ) = E_{t, x₀, x₁} [ ‖ v_θ( (1-t)x₀ + t x₁, t ) − (x₁ − x₀) ‖² ]
```
This is the **conditional flow-matching objective**: cheap, simulation-free regression, directly
analogous in spirit to how denoising score matching reduced an intractable marginal-score target
to a tractable conditional one.

```python
import torch
import torch.nn as nn

class VelocityNet(nn.Module):
    def __init__(self, hidden=128):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(3, hidden), nn.SiLU(),
            nn.Linear(hidden, hidden), nn.SiLU(),
            nn.Linear(hidden, 2),
        )

    def forward(self, x, t):
        t_in = t.view(-1, 1)
        return self.net(torch.cat([x, t_in], dim=-1))

def flow_matching_loss(model, x1, x0=None):
    if x0 is None:
        x0 = torch.randn_like(x1)
    t = torch.rand(x1.shape[0], device=x1.device)
    xt = (1 - t).unsqueeze(-1) * x0 + t.unsqueeze(-1) * x1
    target = x1 - x0
    pred = model(xt, t)
    return ((pred - target) ** 2).sum(-1).mean()

@torch.no_grad()
def flow_matching_sample(model, n_samples=1000, n_steps=100):
    x = torch.randn(n_samples, 2)
    dt = 1.0 / n_steps
    for i in range(n_steps):
        t = torch.full((n_samples,), i * dt)
        x = x + model(x, t) * dt            # forward Euler on dx/dt = v_theta(x,t)
    return x
```

## 5. Flow Matching vs. the Week 2 SDE Sampler
Both are continuous-time generative frameworks producing samples via an ODE/SDE integration; the
flow-matching ODE sampler above is structurally identical to the Week 2 probability-flow ODE
pointer, but flow matching allows a much broader family of probability paths (not only ones
induced by a specific diffusion SDE, e.g., optimal-transport paths that can be shorter/straighter
than a diffusion-induced path, which in practice can allow accurate sampling in fewer integration
steps) and a strictly simpler, simulation-free training objective. Continuous normalizing flows
are the general model family (any ODE-based generative model) that flow matching trains
efficiently; flow matching is a *training method*, not a different model class from CNFs.

## 6. In-Class/Lab Exercise
On the Week 2 toy 2-D Gaussian-mixture data (now with each component labeled by a class $c$),
train a conditional score network with 15% condition dropout, and sample at $w \in \{1, 3, 7\}$,
visualizing how samples concentrate toward each mixture component's mean as $w$ increases. Then
train a `VelocityNet` with `flow_matching_loss` on the same data and compare its training curve's
smoothness and the sampler's required `n_steps` for visually comparable sample quality against the
Week 2 reverse-SDE sampler.
