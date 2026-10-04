# Week 3 Summary — Convolutional Neural Networks I: Convolution Arithmetic and Pooling

**Key takeaways:**
- The convolution output-size formula, $O = \lfloor (I + 2P - K)/S \rfloor + 1$, relates input
  size, padding, kernel size, and stride to output size.
- Receptive field grows with stacked convolutional layers, letting deeper layers "see" larger
  regions of the original input even though each layer is still locally connected.
- Multi-channel convolution extends a single 2D kernel to a 3D kernel per output filter
  (input channels × kernel height × kernel width), with one bias per output filter.
- Parameter sharing is the structural reason a convolutional layer's parameter count does not
  scale with image size, unlike a fully-connected layer on the same input.

**You should now be able to:** compute convolution output size and receptive field by hand;
implement multi-channel convolution and pooling with `nn.Conv2d`/`nn.MaxPool2d`; state the exact
CNN-vs-MLP parameter-count comparison for a given image size.

**Next week:** convolutional neural networks II — the LeNet-to-ResNet architecture evolution, and
building a CNN on CIFAR-10/Fashion-MNIST.
