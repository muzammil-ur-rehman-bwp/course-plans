# Week 10 — Lecture Content: Autoencoders

## 1. The Encoder-Decoder Architecture for Reconstruction

An autoencoder learns to reconstruct its own input through a bottleneck:

```python
import torch
import torch.nn as nn

class Autoencoder(nn.Module):
    def __init__(self, input_dim=784, latent_dim=32):
        super().__init__()
        self.encoder = nn.Sequential(
            nn.Linear(input_dim, 256), nn.ReLU(),
            nn.Linear(256, latent_dim),
        )
        self.decoder = nn.Sequential(
            nn.Linear(latent_dim, 256), nn.ReLU(),
            nn.Linear(256, input_dim), nn.Sigmoid(),   # pixel values in [0, 1]
        )

    def forward(self, x):
        z = self.encoder(x)
        x_hat = self.decoder(z)
        return x_hat, z

model = Autoencoder()
criterion = nn.MSELoss()
# loss = criterion(x_hat, x)  — reconstruct the input itself; no labels needed
```

The bottleneck layer (`latent_dim=32` above, versus `input_dim=784`) forces the network to
compress the input into a lower-dimensional representation and then reconstruct from it — this
compression is where useful structure gets learned.

## 2. Dimensionality Reduction

Once trained, the encoder alone can be used to map high-dimensional inputs to their compact latent
representation, usable for visualization, clustering, or as input features to another model:

```python
model.eval()
with torch.no_grad():
    _, z = model(x_batch)   # z: (batch, latent_dim) — the learned low-dimensional representation
```

## 3. Denoising Autoencoders

A denoising autoencoder is trained to reconstruct a *clean* target from a *corrupted* input,
which encourages the network to learn more robust features than plain reconstruction would:

```python
def add_noise(x, std=0.3):
    return x + std * torch.randn_like(x)

for x, _ in train_loader:
    x_clean = x.view(x.size(0), -1)
    x_noisy = add_noise(x_clean)
    x_hat, _ = model(x_noisy)
    loss = criterion(x_hat, x_clean)   # reconstruct the CLEAN target from the NOISY input
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
```

## 4. Connection to PCA

Consider a **linear** autoencoder: both the encoder and decoder are a single linear layer with no
activation function, trained with squared-error reconstruction loss. It can be shown that the
optimal solution's latent subspace is spanned by the same directions as the top $k$ principal
components found by PCA (where $k$ is the latent dimension) — both are solving the same
"best linear low-rank reconstruction" problem, just via different algorithms (eigendecomposition
for PCA vs. gradient descent for the autoencoder). Adding non-linear activations lets the
autoencoder learn a non-linear manifold that a strictly linear method like PCA cannot represent.

```python
linear_autoencoder = nn.Sequential(
    nn.Linear(784, 32, bias=False),   # encoder: a linear projection, like PCA's projection
    nn.Linear(32, 784, bias=False),   # decoder: a linear reconstruction
)
```

## 5. In-Class Exercise

Explain, in two to three sentences, why a *linear* autoencoder with squared-error loss cannot
outperform PCA at linear dimensionality reduction, and what adding a ReLU activation to the
encoder and decoder buys beyond PCA's capability.
