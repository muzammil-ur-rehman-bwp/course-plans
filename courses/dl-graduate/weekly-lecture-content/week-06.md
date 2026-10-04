# Week 6 — Lecture Content: Normalizing Flows and the Diffusion Forward Process

## 1. The Change-of-Variables Formula for Densities

Let $Z$ have a simple, known density $p_Z(z)$ (e.g., a standard Gaussian), and let $X = f(Z)$ for
an invertible, differentiable $f$. The change-of-variables formula gives $X$'s density exactly:

$$
p_X(x) = p_Z\big(f^{-1}(x)\big)\,\left|\det\frac{\partial f^{-1}(x)}{\partial x}\right|
= p_Z(z)\,\left|\det\frac{\partial f(z)}{\partial z}\right|^{-1}, \quad z = f^{-1}(x).
$$

The Jacobian-determinant factor corrects for how $f$ locally expands or contracts volume: where
$f$ stretches space, probability mass is spread thinner (density decreases); where it compresses
space, density increases. A **normalizing flow** is a deep generative model built by composing
many simple invertible layers $f = f_K \circ \cdots \circ f_1$, so that (a) **sampling** is just
$x = f(z)$ for $z\sim p_Z$, and (b) **exact density evaluation** is tractable via the formula
above, because the log-determinant of a composition is the sum of each layer's
log-determinant: $\log\left|\det \frac{\partial f}{\partial z}\right| = \sum_{k=1}^K
\log\left|\det\frac{\partial f_k}{\partial z_{k-1}}\right|$ — this is the central reason flow
layers are engineered to have cheaply computable Jacobian determinants (e.g., triangular or
diagonal Jacobians).

## 2. A 1-D Worked Example: The Affine Flow

Let $Z\sim\mathcal N(0,1)$ and $f(z) = e^{s}z + t$ (an affine map with scale parameter $s$ and
shift $t$; $f$ is invertible for any finite $s$). Then $f^{-1}(x) = (x-t)e^{-s}$ and
$\left|\frac{df}{dz}\right| = e^{s}$, so

$$
p_X(x) = p_Z\big((x-t)e^{-s}\big)\, e^{-s}
= \frac{1}{\sqrt{2\pi}}\exp\!\left(-\frac{(x-t)^2 e^{-2s}}{2}\right) e^{-s},
$$

which is exactly $\mathcal N(t, e^{2s})$ — an affine flow of a standard Gaussian is Gaussian with
shifted mean and rescaled variance, confirmed directly by the formula. Stacking several affine
layers with data-dependent (neural-network-parameterized) $s,t$ per layer, interleaved with simple
fixed nonlinear invertible maps, is how real flow architectures reshape a Gaussian into
far more complex, multi-modal densities while retaining exact, tractable likelihood.

```python
import torch

class AffineFlow(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.log_scale = torch.nn.Parameter(torch.zeros(()))
        self.shift = torch.nn.Parameter(torch.zeros(()))

    def forward(self, z):
        x = torch.exp(self.log_scale) * z + self.shift
        log_det = self.log_scale                       # d/dz [e^s z + t] = e^s ; log|.| = s
        return x, log_det

    def log_prob(self, x):
        z = (x - self.shift) * torch.exp(-self.log_scale)
        log_pz = -0.5 * z ** 2 - 0.5 * torch.log(torch.tensor(2 * torch.pi))
        return log_pz - self.log_scale                  # log p_X(x) = log p_Z(z) - log_det

# fit by maximum likelihood on 1-D bimodal-ish data (a single affine flow can only fit a Gaussian;
# this illustrates the mechanics, a real flow stacks many invertible layers for multimodal data)
flow = AffineFlow()
optimizer = torch.optim.Adam(flow.parameters(), lr=0.05)
data = torch.randn(512) * 2.0 + 3.0   # true distribution: N(3, 4)
for step in range(300):
    optimizer.zero_grad()
    loss = -flow.log_prob(data).mean()
    loss.backward()
    optimizer.step()
print(f"learned mean={flow.shift.item():.2f}  learned std={torch.exp(flow.log_scale).item():.2f}")
```

## 3. The Diffusion Forward Process

A diffusion model defines a **fixed** (non-learned) forward process: a Markov chain that
progressively corrupts data $x_0$ with Gaussian noise over $T$ steps,

$$
q(x_t \mid x_{t-1}) = \mathcal N\!\left(x_t;\ \sqrt{1-\beta_t}\,x_{t-1},\ \beta_t I\right),
$$

where $\{\beta_t\}_{t=1}^T$ is a fixed, small, increasing noise schedule ($\beta_t \in (0,1)$). As
$t \to T$, $x_T$ becomes (approximately) pure Gaussian noise.

**Closed-form marginal.** Because each step is Gaussian and linear in the previous state, the
marginal distribution of $x_t$ given $x_0$ — skipping every intermediate step — is available in
closed form. Define $\alpha_t = 1-\beta_t$ and $\bar\alpha_t = \prod_{s=1}^t \alpha_s$. Then

$$
q(x_t\mid x_0) = \mathcal N\!\left(x_t;\ \sqrt{\bar\alpha_t}\,x_0,\ (1-\bar\alpha_t)I\right),
\qquad\text{equivalently}\qquad
x_t = \sqrt{\bar\alpha_t}\,x_0 + \sqrt{1-\bar\alpha_t}\,\epsilon,\quad \epsilon\sim\mathcal N(0,I).
$$

This closed form is what makes diffusion training tractable: sampling a noisy $x_t$ at any
timestep $t$ requires only one sampling operation, not $t$ sequential forward steps.

```python
import torch

def make_beta_schedule(T, beta_start=1e-4, beta_end=0.02):
    return torch.linspace(beta_start, beta_end, T)

def forward_diffusion_sample(x0, t, betas):
    alphas = 1.0 - betas
    alpha_bars = torch.cumprod(alphas, dim=0)
    alpha_bar_t = alpha_bars[t].view(-1, *([1] * (x0.dim() - 1)))
    noise = torch.randn_like(x0)
    x_t = alpha_bar_t.sqrt() * x0 + (1 - alpha_bar_t).sqrt() * noise
    return x_t, noise

T = 200
betas = make_beta_schedule(T)
x0 = torch.randn(4, 1) * 0.1 + 2.0          # toy 1-D "data"
for t in [0, 50, 100, 199]:
    x_t, _ = forward_diffusion_sample(x0, torch.tensor([t] * 4), betas)
    print(f"t={t:3d}  mean={x_t.mean().item():.3f}  std={x_t.std().item():.3f}")
```

## 4. In-Class Exercise

Using the 1-D affine-flow formula, derive $p_X(x)$ when $f(z) = z^3$ for $z>0$ (monotonic, hence
invertible on this domain) and $Z\sim\text{Uniform}(0,1)$, and confirm it integrates to 1 over
$(0,1)$.
