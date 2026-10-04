# Week 12 Lecture Plan — Introduction to Deep Learning
## Topic: Generative Models II — Generative Adversarial Networks

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the GAN minimax game between generator and discriminator. (*Understand*)
2. Apply the alternating adversarial training loop in PyTorch. (*Apply*)
3. Analyze common GAN failure modes (mode collapse, training instability) from loss/sample
   evidence. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | From VAE to GAN | Two different answers to "how do we generate realistic new samples?" |
| 0:15–0:45 | The minimax game | Generator maps noise to samples; discriminator classifies real vs. fake; the minimax objective |
| 0:45–1:05 | The adversarial training loop | Alternating discriminator and generator updates, live-coded structure |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Training dynamics & failure modes | Mode collapse; oscillating/diverging losses; why GAN losses alone are a poor training-progress signal |
| 1:40–2:00 | Live demo | Simple GAN on MNIST/Fashion-MNIST; inspecting generated samples over training |

### Materials/Equipment
- Live-coding environment, PyTorch

### Formative Check (in-class)
Given a generator loss that is decreasing steadily toward zero while the discriminator loss is
also near zero, diagnose what is likely happening and why this is not healthy GAN training.

### Link to Lab/Assessment
Lab 12: Build and train a simple GAN on MNIST/Fashion-MNIST; track generator/discriminator losses
and generated-sample quality across training; identify any failure modes observed.
