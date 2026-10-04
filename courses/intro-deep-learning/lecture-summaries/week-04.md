# Week 4 Summary — Convolutional Neural Networks II: Architecture Evolution

**Key takeaways:**
- LeNet established the conv/pool/dense pattern; AlexNet scaled it up with depth, ReLU, and
  dropout; VGG standardized on small 3×3 kernels stacked very deep.
- Very deep plain networks suffer from a degradation problem: adding layers can make training
  harder, not easier, even though a deeper network can always represent what a shallower one does.
- ResNet's skip connections let a layer learn a residual correction to an identity mapping, which
  is easier to learn than a full transformation from scratch, and keeps gradients flowing to early
  layers.
- A CNN with a residual block trains with the same autograd-based loop from Week 1 — only the
  architecture changed.

**You should now be able to:** describe the LeNet→AlexNet→VGG→ResNet progression; explain why skip
connections address the degradation problem; implement a residual block and train a small CNN on
CIFAR-10/Fashion-MNIST.

**Next week:** training deep networks at scale — data augmentation, learning rate schedules, and
transfer learning.
