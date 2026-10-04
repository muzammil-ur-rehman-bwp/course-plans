# Week 2 — Lecture Content: Score-Based Generative Models — The SDE Formulation

## 1. From a Discrete Chain to a Continuous Process
The graduate course's DDPM forward process is a discrete Markov chain:
$x_t = \sqrt{1-\beta_t}\,x_{t-1} + \sqrt{\beta_t}\,\epsilon_t$, $\epsilon_t \sim \mathcal N(0,I)$,
for $t=1,\dots,T$. As $T \to \infty$ with $\beta_t \to \beta(t)\Delta t$ for a fixed step size
$\Delta t = 1/T$, this chain converges to a continuous-time stochastic process described by a
**stochastic differential equation (SDE)**. The continuous-time view is strictly more general: it
lets us choose the forward noising schedule as a continuous function $\beta(t)$, reason about the
process at any real-valued $t$, and — crucially — invokes a general time-reversal theorem that
tells us exactly how to reverse *any* such process, not just the one specific discretization DDPM
happened to use.

## 2. The Forward SDE (Variance-Preserving)
The **Variance-Preserving (VP) SDE** is:
```
dx = -1/2 · β(t) x dt + sqrt(β(t)) dw
```
where $w$ is a standard Wiener process (Brownian motion) and $\beta(t) > 0$ is a noise schedule
increasing in $t$. Discretizing with step size $\Delta t$ via the Euler-Maruyama method
(the SDE analogue of Euler's method for ODEs — replace $dx$ with a finite increment, $dt$ with
$\Delta t$, and $dw$ with $\sqrt{\Delta t}\,\epsilon$, $\epsilon\sim\mathcal N(0,I)$):
```
x_{t+Δt} = x_t - 1/2 · β(t) x_t Δt + sqrt(β(t) Δt) ε
         = (1 - 1/2 β(t)Δt) x_t + sqrt(β(t)Δt) ε
```
Setting $\beta_t := \beta(t)\Delta t$ (the discrete-chain noise schedule) and using
$\sqrt{1-\beta_t} \approx 1 - \tfrac12\beta_t$ for small $\beta_t$, this is exactly DDPM's forward
step $x_t \approx \sqrt{1-\beta_t}\,x_{t-1} + \sqrt{\beta_t}\,\epsilon$. **DDPM's forward process is
the Euler-Maruyama discretization of the VP-SDE.** The VP-SDE also admits a closed-form Gaussian
marginal directly from $x_0$ (exactly as DDPM's closed-form $q(x_t\mid x_0)$ does), which Section 4
uses.

## 3. The Reverse-Time SDE
Anderson's time-reversal result (a classical result in stochastic-process theory, stated here and
used, not re-derived from measure theory) says: if $x$ evolves forward according to
$dx = f(x,t)\,dt + g(t)\,dw$, then running time backwards, the same process is described by
```
dx = [f(x,t) - g(t)² ∇ₓ log p_t(x)] dt + g(t) dw̄
```
where $d\bar w$ is a Wiener process run backwards in time and $p_t(x)$ is the marginal density of
$x_t$ under the forward process. The term $\nabla_x \log p_t(x)$ is the **score function** — the
gradient of the log-density with respect to $x$ at noise level $t$. This equation says something
remarkable: *if we know the score function at every noise level*, we can simulate this reverse SDE
starting from $x_T \sim \mathcal N(0,I)$ (the forward process's known, tractable limiting
distribution) and arrive at a sample $x_0 \sim p_0$ — i.e., a sample from the data distribution.
Sampling has been reduced to a single unknown: the score.

## 4. Score Matching
The true score $\nabla_x \log p_t(x)$ is unknown (we don't have $p_t$ in closed form — only
samples). We train a neural network $s_\theta(x,t)$ to approximate it. The key trick is
**denoising score matching**: the VP-SDE's closed-form marginal gives us $p_t(x_t \mid x_0)$ in
closed form (a Gaussian, exactly as in DDPM), and for a Gaussian conditional, its score is
analytically known:
```
p_t(x_t | x_0) = N(x_t; √(ᾱ_t) x_0, (1-ᾱ_t) I)
⟹  ∇_{x_t} log p_t(x_t | x_0) = -(x_t - √(ᾱ_t) x_0) / (1 - ᾱ_t) = -ε / √(1-ᾱ_t)
```
(writing $x_t = \sqrt{\bar\alpha_t}\,x_0 + \sqrt{1-\bar\alpha_t}\,\epsilon$, exactly DDPM's
closed-form marginal). The denoising score-matching objective trains $s_\theta$ to match this
*conditional* score (which is tractable) rather than the true marginal score (which is not),
justified by the standard score-matching identity that the two share the same minimizer in
expectation:
```
L(θ) = E_{t, x₀, ε} [ λ(t) · ‖ s_θ(x_t, t) - ∇_{x_t} log p_t(x_t | x₀) ‖² ]
     = E_{t, x₀, ε} [ λ(t) / (1-ᾱ_t) · ‖ s_θ(x_t, t) · (-√(1-ᾱ_t)) - ε ‖² ]   (up to rescaling)
```
Reparameterizing $s_\theta(x_t,t) := -\epsilon_\theta(x_t,t)/\sqrt{1-\bar\alpha_t}$ and choosing the
weighting $\lambda(t) = 1-\bar\alpha_t$ recovers, exactly, **DDPM's simplified training loss**
$\mathbb E_{t,x_0,\epsilon}[\lVert \epsilon_\theta(x_t,t) - \epsilon\rVert^2]$. This is the central
result of the week: **discrete-time DDPM is denoising score matching for the VP-SDE, under a
specific reparameterization and loss weighting — not a different model from score-based SDEs, a
special case of one.**

```python
import torch
import torch.nn as nn

class ScoreNet(nn.Module):
    """Toy score network s_theta(x, t) for 2-D data."""
    def __init__(self, hidden=128):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(3, hidden), nn.SiLU(),   # input: x (2-D) concatenated with scalar t
            nn.Linear(hidden, hidden), nn.SiLU(),
            nn.Linear(hidden, 2),
        )

    def forward(self, x, t):
        t_in = t.view(-1, 1).expand(x.shape[0], 1)
        return self.net(torch.cat([x, t_in], dim=-1))

def beta_t(t, beta_min=0.1, beta_max=20.0):
    return beta_min + t * (beta_max - beta_min)

def marginal_prob(x0, t, beta_min=0.1, beta_max=20.0):
    """Closed-form VP-SDE marginal mean/std at time t in [0,1]."""
    log_mean_coeff = -0.25 * t**2 * (beta_max - beta_min) - 0.5 * t * beta_min
    mean = torch.exp(log_mean_coeff).unsqueeze(-1) * x0
    std = torch.sqrt(1.0 - torch.exp(2.0 * log_mean_coeff)).unsqueeze(-1)
    return mean, std

def score_matching_loss(model, x0):
    t = torch.rand(x0.shape[0], device=x0.device) * (1.0 - 1e-3) + 1e-3
    mean, std = marginal_prob(x0, t)
    eps = torch.randn_like(x0)
    xt = mean + std * eps
    target_score = -eps / std                      # closed-form conditional score
    pred_score = model(xt, t)
    return ((pred_score - target_score) ** 2).sum(-1).mean()
```

## 5. Sampling via Euler-Maruyama on the Reverse SDE
```python
@torch.no_grad()
def reverse_sde_sample(model, n_samples=1000, n_steps=1000, beta_min=0.1, beta_max=20.0):
    x = torch.randn(n_samples, 2)              # x_T ~ N(0, I)
    dt = 1.0 / n_steps
    for i in range(n_steps, 0, -1):
        t = torch.full((n_samples,), i * dt)
        beta = beta_t(t, beta_min, beta_max).unsqueeze(-1)
        score = model(x, t)
        drift = -0.5 * beta * x - beta * score   # f(x,t) - g(t)^2 * score
        noise = torch.randn_like(x) if i > 1 else torch.zeros_like(x)
        x = x - drift * dt + torch.sqrt(beta * dt) * noise
    return x
```
Note the sign: simulating the reverse SDE runs $t$ from $T$ down to $0$, so the Euler-Maruyama
update subtracts $\mathrm{drift}\cdot dt$ (moving backwards in time) — this is the one place the
reverse-time discretization differs mechanically from the forward one in Section 2.

## 6. Probability-Flow ODE (Pointer)
The same marginals $p_t(x)$ are also produced by a **deterministic** ODE,
$dx = [f(x,t) - \tfrac12 g(t)^2 \nabla_x\log p_t(x)]\,dt$ (half the diffusion term of the reverse
SDE, no noise) — the probability-flow ODE. It samples the same distribution with no stochasticity
once $x_T$ is fixed, and is the starting point for the fast, few-step samplers surveyed in
Week 12.

## 7. In-Class/Lab Exercise
Train `ScoreNet` via `score_matching_loss` on samples from a 2-D two-component Gaussian mixture.
Run `reverse_sde_sample` and overlay the generated samples on the true mixture's density contours.
Verify that increasing `n_steps` improves sample fidelity and that `n_steps` too small produces
visibly biased samples — the discretization-error tradeoff the Week 12 fast-sampler survey
revisits.
