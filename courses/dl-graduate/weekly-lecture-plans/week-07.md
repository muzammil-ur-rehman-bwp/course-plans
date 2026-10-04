# Week 7 Lecture Plan — Deep Learning (Graduate)
## Topic: Advanced Generative Models II — The Diffusion Reverse Process and Training

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Derive the diffusion reverse process and the simplified (DDPM-style) training objective.
   (*Analyze*)
2. Implement the simplified training loss and the sampling procedure on toy data. (*Apply*)
3. Contrast diffusion models with VAEs and GANs on training stability and sampling cost.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 6's forward process; today: learning to reverse it |
| 0:15–0:45 | The reverse process | The learned Gaussian reverse step; parameterizing the mean via a learned noise predictor $\epsilon_\theta$ |
| 0:45–1:05 | The simplified training objective | The DDPM-style loss $\mathbb{E}\|\epsilon-\epsilon_\theta(x_t,t)\|^2$; why it is tractable |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Sampling | The iterative reverse sampling loop from pure noise to a generated sample |
| 1:40–2:00 | Comparison | Diffusion vs. GAN vs. VAE: training stability, sample quality, sampling-speed trade-offs |

### Materials/Equipment
- Live-coding environment, PyTorch
- Slide diagram: reverse sampling chain from $x_T$ to $x_0$

### Formative Check (in-class)
Students write, from memory, the simplified DDPM loss and identify which quantity the network
$\epsilon_\theta$ is trained to predict and why that choice makes training stable and simple.

### Link to Lab/Assessment
Lab 7: Implementing the simplified diffusion training loss and sampling loop on a 2-D synthetic
dataset (see `lab-manuals/lab-07.md`).
