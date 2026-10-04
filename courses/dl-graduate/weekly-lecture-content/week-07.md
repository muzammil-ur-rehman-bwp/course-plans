# Week 7 — Lecture Content: The Diffusion Reverse Process and Training

## 1. The Reverse Denoising Process

The forward process (Week 6) is fixed and requires no learning. Generation requires its inverse: a
**learned** reverse process that starts from pure noise $x_T \sim \mathcal N(0,I)$ and
progressively denoises it back toward the data distribution. Each reverse step is modeled as a
Gaussian,

$$
p_\theta(x_{t-1}\mid x_t) = \mathcal N\big(x_{t-1};\ \mu_\theta(x_t,t),\ \Sigma_\theta(x_t,t)\big),
$$

where $\Sigma_\theta$ is often fixed to a schedule-determined constant (not learned), and the
learned mean $\mu_\theta$ is reparameterized in terms of a **noise-prediction network**
$\epsilon_\theta(x_t,t)$ that predicts the noise $\epsilon$ which was added to produce $x_t$ from
$x_0$ (per Week 6's forward-process closed form):

$$
\mu_\theta(x_t,t) = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{\beta_t}{\sqrt{1-\bar\alpha_t}}\,
\epsilon_\theta(x_t,t)\right).
$$

This reparameterization (predict the noise, not the mean directly) is what makes the next
section's objective both simple and effective in practice.

## 2. The Simplified Training Objective

The full variational bound on the data log-likelihood (from treating the reverse process as a
hierarchical latent-variable model, analogous in spirit to the VAE's ELBO already known from the
undergraduate course) reduces, after Ho et al.'s simplification, to a single, easy-to-optimize
regression loss. Sample a data point $x_0$, a random timestep $t\sim\text{Uniform}\{1,\ldots,T\}$,
and noise $\epsilon\sim\mathcal N(0,I)$; form the noisy input via the Week 6 closed form $x_t =
\sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\epsilon$; and train $\epsilon_\theta$ to recover the
noise that was actually added:

$$
\mathcal{L}_{\text{simple}}(\theta) = \mathbb{E}_{t,\,x_0,\,\epsilon}\Big[\big\|\epsilon -
\epsilon_\theta(x_t, t)\big\|^2\Big].
$$

This is tractable (a plain regression loss, no intractable integrals) precisely because the
forward process's closed form lets every training step construct a valid $(x_t, t, \epsilon)$
triple in one sampling operation, with the ground-truth target ($\epsilon$) known exactly, by
construction.

```python
import torch
import torch.nn as nn

def diffusion_training_loss(eps_model, x0, betas):
    T = betas.size(0)
    alphas = 1.0 - betas
    alpha_bars = torch.cumprod(alphas, dim=0)

    B = x0.size(0)
    t = torch.randint(0, T, (B,), device=x0.device)
    alpha_bar_t = alpha_bars[t].view(-1, *([1] * (x0.dim() - 1)))
    noise = torch.randn_like(x0)
    x_t = alpha_bar_t.sqrt() * x0 + (1 - alpha_bar_t).sqrt() * noise

    predicted_noise = eps_model(x_t, t.float())
    return nn.functional.mse_loss(predicted_noise, noise)

class TinyNoisePredictor(nn.Module):
    """A minimal eps_theta(x_t, t) for 1-D toy data."""
    def __init__(self, hidden=64):
        super().__init__()
        self.net = nn.Sequential(nn.Linear(2, hidden), nn.ReLU(),
                                  nn.Linear(hidden, hidden), nn.ReLU(), nn.Linear(hidden, 1))

    def forward(self, x_t, t):
        t_norm = (t / 200.0).view(-1, 1)
        return self.net(torch.cat([x_t, t_norm], dim=1))
```

## 3. The Sampling Procedure

To generate a sample, start from pure noise and iteratively apply the learned reverse step:

$$
x_{t-1} = \frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{\beta_t}{\sqrt{1-\bar\alpha_t}}\,
\epsilon_\theta(x_t,t)\right) + \sigma_t z, \qquad z\sim\mathcal N(0,I)\ (z=0 \text{ at } t=1),
$$

where $\sigma_t^2$ is a schedule-determined variance (e.g., $\sigma_t^2=\beta_t$). Repeating this
from $t=T$ down to $t=1$ turns pure noise into a sample from (approximately) the learned data
distribution — note this requires $T$ sequential network evaluations, in contrast to a GAN's or
VAE's single forward pass.

```python
@torch.no_grad()
def sample(eps_model, betas, shape):
    T = betas.size(0)
    alphas = 1.0 - betas
    alpha_bars = torch.cumprod(alphas, dim=0)
    x = torch.randn(shape)
    for t in reversed(range(T)):
        t_batch = torch.full((shape[0],), t, dtype=torch.float32)
        eps_hat = eps_model(x, t_batch)
        alpha_t, alpha_bar_t, beta_t = alphas[t], alpha_bars[t], betas[t]
        mean = (x - beta_t / (1 - alpha_bar_t).sqrt() * eps_hat) / alpha_t.sqrt()
        if t > 0:
            x = mean + beta_t.sqrt() * torch.randn_like(x)
        else:
            x = mean
    return x
```

## 4. Diffusion vs. GAN vs. VAE

| | VAE | GAN | Diffusion |
|---|---|---|---|
| Generation | one encode/decode pass | one generator pass | $T$ sequential denoising steps |
| Training signal | ELBO (recon. + KL) | adversarial minimax game | simple noise-regression MSE |
| Training stability | generally stable | can be unstable (mode collapse, oscillation) | generally stable (regression, not a minimax game) |
| Sample quality (typical) | often blurrier | can be sharp but mode-limited | typically sharp and diverse |
| Sampling cost | cheap (1 pass) | cheap (1 pass) | expensive ($T$ passes, though fast samplers exist) |

Diffusion models trade sampling-time cost for training stability and sample diversity/quality
relative to GANs, and for sample sharpness relative to a plain VAE's reconstruction objective.

## 5. In-Class Exercise

Given $\alpha_t=0.98$, $\bar\alpha_t=0.80$, $\beta_t=0.02$, $x_t=1.5$, and a predicted
$\epsilon_\theta(x_t,t)=0.3$, compute the reverse-step mean $\mu_\theta(x_t,t)$ by hand.
