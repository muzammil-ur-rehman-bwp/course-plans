# Week 16 — Lecture Content: Capstone Presentations + Course Review

## 1. The Full Course Map

```
Prerequisite (Introduction to Artificial Neural Networks):
  Perceptron -> MLP -> Backpropagation (derived) -> Optimizers & Regularization (derived)
       |
       v
Week 1:  PyTorch as the course's framework (tensors, autograd, nn.Module, training loop)
Week 2:  Deep nets in practice (init, batch norm, dropout, gradient flow) -- revisited at depth
       |
       v
Weeks 3-5:  Convolutional Neural Networks
  convolution arithmetic -> LeNet/AlexNet/VGG/ResNet -> augmentation/schedules/transfer learning
       |
       v
Weeks 6-9:  Sequence Models & Attention
  RNN limits -> LSTM/GRU -> seq2seq -> attention -> midterm -> Transformer survey
       |
       v
Weeks 10-12:  Generative Models
  autoencoders -> VAEs (reparameterization, ELBO) -> GANs (minimax game)
       |
       v
Weeks 13-15:  Practice, Optimization & Ethics
  AdamW/label smoothing/mixed precision -> end-to-end workflows & debugging -> bias & compute cost
       |
       v
Week 16:  Capstone presentations + course review
```

## 2. A Unifying Observation

Every architecture studied this semester — CNN, LSTM/GRU, Transformer, autoencoder, VAE, GAN —
is trained with the *same* underlying machinery: autograd-based gradient computation and an
optimizer (Adam/AdamW), exactly as established in the prerequisite course and reinforced in Week 1.
What changed across the semester was never the training mechanism; it was always the
**architecture** — the specific way layers are arranged to exploit the structure of a particular
kind of data (spatial for CNNs, sequential for LSTM/GRU/attention, unsupervised-reconstructive for
autoencoders/VAEs, adversarial for GANs).

## 3. Capstone Presentations

The remaining class time is used for student capstone presentations (see
`presentations/capstone-presentation-template.md` and `assignments/capstone-rubric.md`). Each
presentation should make clear: the problem, the architecture and framework choice, the training
setup, the results, and — critically — a design trade-off the team evaluated and what they
learned from it, however the outcome turned out.

## 4. Where This Leads Next

The architectural survey in this course (full depth on CNNs/RNNs, an introductory survey of
Transformers, and introductory generative models) is the standard foundation for:
- A dedicated advanced deep learning course (full Transformer implementation, more advanced
  generative models, and training at larger scale).
- Graduate-level deep learning and specialized architecture courses.
- Applied project work in computer vision, NLP, or generative modeling, now that the architectural
  vocabulary and PyTorch fluency from this semester are in place.

## 5. In-Class Exercise

For each student/pair, after their presentation: name one design choice you would revisit with
more time or compute, and explain what you would expect to change as a result.
