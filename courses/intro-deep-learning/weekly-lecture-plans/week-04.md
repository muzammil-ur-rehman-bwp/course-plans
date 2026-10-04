# Week 4 Lecture Plan — Introduction to Deep Learning
## Topic: Convolutional Neural Networks II — Architecture Evolution and a CNN on CIFAR-10

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the architectural progression from LeNet to AlexNet to VGG. (*Understand*)
2. Analyze the degradation problem in very deep networks and why ResNet's skip connections address
   it. (*Analyze*)
3. Apply a CNN architecture (optionally including a residual block) to train an image classifier
   on CIFAR-10/Fashion-MNIST in PyTorch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | LeNet recap | The original conv/pool/dense pattern as the starting template |
| 0:15–0:35 | AlexNet & VGG | Depth, ReLU, dropout at scale (AlexNet); uniform small kernels, very deep stacks (VGG) |
| 0:35–0:45 | Break | — |
| 0:45–1:15 | The degradation problem & ResNet | Why adding layers can make deep networks harder, not easier, to train; the residual/skip connection as a fix; identity mapping intuition |
| 1:15–1:35 | Residual block implementation | `nn.Conv2d` + skip connection with addition, live-coded |
| 1:35–2:00 | Building the lab architecture | Assembling a small CNN with one residual block for CIFAR-10/Fashion-MNIST |

### Materials/Equipment
- Live-coding environment, PyTorch, `torchvision.datasets.CIFAR10`
- Diagram handout: LeNet/AlexNet/VGG/ResNet block diagrams side by side

### Formative Check (in-class)
Explain, in two to three sentences, why a residual connection makes it easier for a layer to learn
"do nothing" (an identity mapping) than a plain stacked layer would.

### Link to Lab/Assessment
Lab 4: Build and train a small CNN (with at least one residual block) on CIFAR-10 or
Fashion-MNIST; plot training/validation curves.

### Assessment Note
**Assignment 1** (CNN arithmetic and architectures) is assigned this week.
