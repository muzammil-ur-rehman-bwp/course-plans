# Week 11 — Lecture Content: Generative Models I — Variational Autoencoders

## 1. The Generative Modeling Problem

Week 10's autoencoder reconstructs *given* inputs well, but its latent space has no guarantee of
being well-structured for sampling: picking a random point in latent space and decoding it may
produce nothing realistic. Generative modeling asks for more — a model from which genuinely new,
realistic samples can be drawn.

## 2. The Probabilistic Encoder

A VAE's encoder outputs the parameters of a distribution over latent codes — typically a Gaussian
mean $\mu$ and log-variance $\log \sigma^2$ — rather than a single point:

```python
import torch
import torch.nn as nn

class VAEEncoder(nn.Module):
    def __init__(self, input_dim=784, hidden_dim=256, latent_dim=20):
        super().__init__()
        self.fc1 = nn.Linear(input_dim, hidden_dim)
        self.fc_mu = nn.Linear(hidden_dim, latent_dim)
        self.fc_logvar = nn.Linear(hidden_dim, latent_dim)

    def forward(self, x):
        h = torch.relu(self.fc1(x))
        return self.fc_mu(h), self.fc_logvar(h)
```

## 3. The Reparameterization Trick

Sampling $z \sim \mathcal{N}(\mu, \sigma^2)$ directly is not differentiable with respect to $\mu$
and $\sigma$, which would block backpropagation through the sampling step. The reparameterization
trick rewrites the sample as a deterministic, differentiable function of $\mu$, $\sigma$, and an
independent noise source $\epsilon$:

$$
z = \mu + \sigma \odot \epsilon, \qquad \epsilon \sim \mathcal{N}(0, I)
$$

Now the randomness is isolated in $\epsilon$ (which requires no gradient), and $\partial z /
\partial \mu$ and $\partial z / \partial \sigma$ are both well-defined, so gradients flow through
$z$ back into the encoder exactly as they would through any other differentiable operation.

```python
def reparameterize(mu, logvar):
    std = torch.exp(0.5 * logvar)
    eps = torch.randn_like(std)
    return mu + eps * std
```

## 4. The ELBO Loss

A VAE is trained by maximizing the **Evidence Lower Bound (ELBO)** on the data log-likelihood,
equivalently minimizing its negative:

$$
\mathcal{L}_{\text{ELBO}} = \underbrace{\mathbb{E}_{q(z|x)}[\log p(x|z)]}_{\text{reconstruction term}}
- \underbrace{D_{KL}\big(q(z|x) \,\|\, p(z)\big)}_{\text{KL regularization term}}
$$

With a Gaussian encoder $q(z|x) = \mathcal{N}(\mu, \sigma^2)$ and a standard normal prior
$p(z) = \mathcal{N}(0, I)$, the KL term has a closed form:

$$
D_{KL}\big(\mathcal{N}(\mu,\sigma^2) \,\|\, \mathcal{N}(0,I)\big)
= -\frac{1}{2}\sum_{j=1}^{d}\Big(1 + \log\sigma_j^2 - \mu_j^2 - \sigma_j^2\Big)
$$

```python
import torch.nn.functional as F

def vae_loss(x_hat, x, mu, logvar):
    recon_loss = F.binary_cross_entropy(x_hat, x, reduction='sum')
    kl_loss = -0.5 * torch.sum(1 + logvar - mu.pow(2) - logvar.exp())
    return recon_loss + kl_loss   # minimizing this is equivalent to maximizing the ELBO
```

The reconstruction term pulls the decoder toward accurately reconstructing the input; the KL term
regularizes the latent distribution toward the prior, which is what makes sampling $z \sim
\mathcal{N}(0, I)$ at generation time produce realistic outputs.

## 5. A Simple VAE, End to End

```python
class VAE(nn.Module):
    def __init__(self, input_dim=784, hidden_dim=256, latent_dim=20):
        super().__init__()
        self.encoder = VAEEncoder(input_dim, hidden_dim, latent_dim)
        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, hidden_dim), nn.ReLU(),
            nn.Linear(hidden_dim, input_dim), nn.Sigmoid(),
        )

    def forward(self, x):
        mu, logvar = self.encoder(x)
        z = reparameterize(mu, logvar)
        x_hat = self.decoder(z)
        return x_hat, mu, logvar

# Sampling new images after training:
model.eval()
with torch.no_grad():
    z_sample = torch.randn(16, 20)        # sample directly from the prior N(0, I)
    generated = model.decoder(z_sample)   # decode into new, realistic-looking images
```

## 6. In-Class Exercise

Explain why `z = mu + sigma * eps` can be backpropagated through while `z ~ N(mu, sigma^2)`
sampled directly cannot, and identify exactly which operation in the reparameterized form carries
no gradient.
