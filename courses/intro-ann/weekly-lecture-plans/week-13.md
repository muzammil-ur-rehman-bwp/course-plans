# Week 13 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Convolutional Neural Networks (Basics)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the convolution and pooling operations. (*Understand*)
2. Apply a small CNN architecture to an image classification task using a framework. (*Apply*)
3. Analyze why weight sharing and local receptive fields make CNNs better suited to image data
   than a fully-connected MLP. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & motivation | An MLP on a flattened image ignores 2D spatial structure entirely |
| 0:15–0:45 | The convolution operation | Kernels/filters, stride, padding; hand-computed example |
| 0:45–0:55 | Break | — |
| 0:55–1:20 | Pooling | Max/average pooling; downsampling and translation robustness |
| 1:20–1:45 | Why CNNs suit images | Weight sharing & local receptive fields vs. MLP parameter count |
| 1:45–2:00 | A minimal CNN architecture | conv → pool → conv → pool → dense, with a framework code sketch |

### Materials/Equipment
- Live-coding environment, PyTorch or Keras
- Handout: convolution computed by hand on a small matrix

### Formative Check (in-class)
By hand, compute the output of a 3×3 convolution kernel applied (stride 1, no padding) to a
given 5×5 input matrix, for at least one output position.

### Link to Lab/Assessment
Lab 13: Implement convolution and max-pooling by hand on a small matrix in NumPy; build and train
a small CNN on MNIST/Fashion-MNIST with the framework.
