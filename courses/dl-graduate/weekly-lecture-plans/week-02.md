# Week 2 Lecture Plan — Deep Learning (Graduate)
## Topic: Advanced CNN Architectures

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the ResNet residual formulation's identity-mapping argument at implementation depth.
   (*Understand*)
2. Apply DenseNet's dense-connectivity pattern and depthwise-separable convolutions in PyTorch.
   (*Apply*)
3. Analyze the parameter/FLOP trade-offs of standard vs. depthwise-separable convolutions.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Recap | ResNet/skip-connections as surveyed at the undergraduate level; today we go deeper |
| 0:10–0:35 | ResNet residual formulation in depth | $H(x) = F(x) + x$; identity-mapping argument; brief cross-reference to ANN-graduate's loss-landscape angle (not repeated here) |
| 0:35–1:00 | DenseNet | Dense connectivity, concatenation across a dense block, feature reuse, growth rate |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | Efficiency-focused architectures | Depthwise separable convolutions; MobileNet/EfficientNet-style design motivation; parameter/FLOP comparison |
| 1:40–2:00 | Live demo | Implementing and shape-checking a depthwise separable block against a standard `nn.Conv2d` of matching channels |

### Materials/Equipment
- Live-coding environment, PyTorch (Colab or local with GPU)
- Slide diagrams: residual block, dense block, depthwise separable factorization

### Formative Check (in-class)
Given input/output channel counts and a kernel size, compute by hand the parameter count of a
standard `Conv2d` versus a depthwise separable factorization, and state the approximate reduction
factor.

### Link to Lab/Assessment
Lab 2: Implementing a residual block, a dense block, and a depthwise separable convolution block
in PyTorch, and comparing parameter counts (see `lab-manuals/lab-02.md`).
