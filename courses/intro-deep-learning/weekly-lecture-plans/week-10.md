# Week 10 Lecture Plan — Introduction to Deep Learning
## Topic: Autoencoders

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the encoder-decoder architecture for unsupervised reconstruction. (*Understand*)
2. Apply a denoising autoencoder to reconstruct clean inputs from corrupted ones. (*Apply*)
3. Explain the connection between a linear autoencoder and PCA. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Transition | From supervised seq2seq/attention to unsupervised representation learning |
| 0:15–0:40 | Autoencoder architecture | Encoder → bottleneck (latent) → decoder; reconstruction loss |
| 0:40–1:00 | Dimensionality reduction | Using the bottleneck layer as a learned low-dimensional representation |
| 1:00–1:10 | Break | — |
| 1:10–1:30 | Denoising autoencoders | Corrupting the input, reconstructing the clean target |
| 1:30–1:50 | Connection to PCA | A linear autoencoder with squared-error loss and PCA's top components |
| 1:50–2:00 | Live demo | Small fully-connected autoencoder on MNIST/Fashion-MNIST, latent space visualized |

### Materials/Equipment
- Live-coding environment, PyTorch

### Formative Check (in-class)
Explain, in two to three sentences, why a linear autoencoder's optimal solution is related to
PCA's principal subspace, and what adding non-linear activations buys beyond that.

### Link to Lab/Assessment
Lab 10: Build and train a (denoising) autoencoder on MNIST/Fashion-MNIST; visualize
reconstructions and the latent space.
