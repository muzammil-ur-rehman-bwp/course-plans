# Presentation: Module 1 — Deep Net Practice & Convolutional Neural Networks (Weeks 1–5)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 1: Deep Net Practice & Convolutional Neural Networks
2. **Course framing** — how this course builds on *Introduction to Artificial Neural Networks*
3. **PyTorch tour** — tensors, autograd, `nn.Module`, the canonical training loop
4. **Initialization revisited** — Xavier/Glorot and He, variance-preservation rationale
5. **Batch normalization** — algorithm, train-mode vs. eval-mode statistics
6. **Dropout revisited** — regularization view and implicit-ensembling view
7. **Vanishing/exploding gradients revisited** — concrete mitigations map
8. **Convolution arithmetic** — kernel/stride/padding, output-size formula, worked example
9. **Receptive field & parameter sharing** — growth across layers; exact CNN vs. MLP comparison
10. **LeNet → AlexNet → VGG** — the architectural progression
11. **The degradation problem & ResNet** — skip connections and the identity-mapping argument
12. **Data augmentation & LR schedules** — regularizing transforms; warmup and why it matters
13. **Transfer learning & fine-tuning** — feature extraction vs. fine-tuning, `torchvision.models`

**Speaker notes:** slide 11 (ResNet/skip connections) is this module's conceptual anchor — it is
the first architectural idea students see that directly addresses a training *failure mode*
(degradation) rather than only improving representational capacity; give it real time.
