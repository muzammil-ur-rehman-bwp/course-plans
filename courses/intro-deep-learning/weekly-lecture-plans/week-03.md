# Week 3 Lecture Plan — Introduction to Deep Learning
## Topic: Convolutional Neural Networks I — Convolution Arithmetic and Pooling

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply the convolution output-size formula to compute output and receptive field sizes for a
   given kernel/stride/padding configuration. (*Apply*)
2. Explain pooling's effect on spatial resolution and translation robustness. (*Understand*)
3. Analyze why parameter sharing makes CNNs more parameter-efficient than MLPs on image data, with
   an exact comparison. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Brief CNN mention from the prerequisite course; today goes to full arithmetic depth |
| 0:15–0:45 | Convolution arithmetic | Kernel size, stride, padding; output-size formula derivation; worked numeric example |
| 0:45–1:05 | Receptive field | How receptive field grows with stacked layers; worked example across 2–3 layers |
| 1:05–1:15 | Break | — |
| 1:15–1:35 | Multi-channel convolution | Input channels, output channels/filters; parameter count formula |
| 1:35–1:50 | Pooling | Max/average pooling; downsampling and translation robustness |
| 1:50–2:00 | Parameter sharing, in depth | Exact CNN vs. MLP parameter-count comparison on a concrete image size |

### Materials/Equipment
- Live-coding environment, PyTorch
- Handout: convolution output-size and receptive-field worked examples

### Formative Check (in-class)
Compute the output spatial size of a 32×32×3 input after two successive `Conv2d` layers (kernel
5, stride 1, padding 2) each followed by 2×2 max pooling (stride 2), and compute the receptive
field at the final feature map.

### Link to Lab/Assessment
Lab 3: Compute convolution output sizes and receptive fields by hand for several configurations;
implement multi-channel convolution and pooling with `nn.Conv2d`/`nn.MaxPool2d` and verify shapes.
