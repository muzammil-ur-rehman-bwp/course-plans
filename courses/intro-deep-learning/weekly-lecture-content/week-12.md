# Week 12 — Lecture Content: Generative Models II — Generative Adversarial Networks

## 1. The Minimax Game

A GAN consists of two networks trained against each other:
- The **generator** $G$ maps noise $z \sim p_z$ to a sample $G(z)$ meant to look like real data.
- The **discriminator** $D$ outputs the probability that a given sample is real (vs. generated).

They are trained via the minimax objective:

$$
\min_G \max_D V(D, G) = \mathbb{E}_{x \sim p_{\text{data}}}[\log D(x)] +
\mathbb{E}_{z \sim p_z}[\log(1 - D(G(z)))]
$$

$D$ is trained to maximize this (correctly classify real as real, fake as fake); $G$ is trained to
minimize it (fool $D$ into classifying its output as real).

## 2. Generator and Discriminator Networks

```python
import torch
import torch.nn as nn

class Generator(nn.Module):
    def __init__(self, noise_dim=64, img_dim=784):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(noise_dim, 256), nn.ReLU(),
            nn.Linear(256, img_dim), nn.Tanh(),   # output scaled to [-1, 1]
        )

    def forward(self, z):
        return self.net(z)

class Discriminator(nn.Module):
    def __init__(self, img_dim=784):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(img_dim, 256), nn.LeakyReLU(0.2),
            nn.Linear(256, 1), nn.Sigmoid(),      # output a probability
        )

    def forward(self, x):
        return self.net(x)
```

## 3. The Adversarial Training Loop

```python
import torch.optim as optim

G = Generator()
D = Discriminator()
criterion = nn.BCELoss()
opt_G = optim.Adam(G.parameters(), lr=2e-4, betas=(0.5, 0.999))
opt_D = optim.Adam(D.parameters(), lr=2e-4, betas=(0.5, 0.999))

for real_images in dataloader:
    batch_size = real_images.size(0)
    real_labels = torch.ones(batch_size, 1)
    fake_labels = torch.zeros(batch_size, 1)

    # --- Train Discriminator ---
    opt_D.zero_grad()
    d_real_loss = criterion(D(real_images), real_labels)
    noise = torch.randn(batch_size, 64)
    fake_images = G(noise)
    d_fake_loss = criterion(D(fake_images.detach()), fake_labels)   # detach: don't backprop into G here
    d_loss = d_real_loss + d_fake_loss
    d_loss.backward()
    opt_D.step()

    # --- Train Generator ---
    opt_G.zero_grad()
    noise = torch.randn(batch_size, 64)
    fake_images = G(noise)
    g_loss = criterion(D(fake_images), real_labels)   # G wants D to classify fakes as real
    g_loss.backward()
    opt_G.step()
```

Note the `.detach()` when training $D$ on generated images: it stops gradients from flowing into
$G$ during the discriminator's update, since that update should only change $D$'s parameters.

## 4. Training Dynamics and Failure Modes

Unlike supervised training, a decreasing loss for either network is *not* a reliable progress
signal on its own — a very low discriminator loss can mean $D$ has become too strong for $G$ to
ever fool (stalling $G$'s learning), while a very low generator loss paired with a collapsing
discriminator loss near 0.5 can simply mean both networks have stopped improving in a degenerate
equilibrium.

| Failure mode | Symptom | Likely cause |
|---|---|---|
| Mode collapse | Generated samples lack diversity (many near-identical outputs) | $G$ has found a small set of outputs that reliably fool the current $D$ |
| Training instability | Losses oscillate or diverge rather than settle | Learning rates too high, or $D$/$G$ updated too unevenly |

Because loss curves alone are unreliable, GAN training is diagnosed largely by **inspecting
generated samples directly** over the course of training, in addition to watching the loss curves.

## 5. In-Class Exercise

Given a GAN whose generator loss decreases steadily to near zero while the discriminator loss also
falls to near zero, explain what is likely happening and why this is not evidence of healthy
training.
