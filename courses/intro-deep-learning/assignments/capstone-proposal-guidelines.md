# Capstone Project — Proposal Guidelines

**Due:** Week 11 | **Weight:** 2% of the Capstone grade (20% total course weight)

## What to Submit
A 1-page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Problem statement**: what are you building — an image classifier, a text/time-series
   sequence model, or a generative model — and on what kind of data?
2. **Dataset**: source, size, and format (must be finalized/accessible by the proposal deadline —
   e.g., CIFAR-10, Fashion-MNIST, a public text/time-series dataset, or another small, publicly
   available dataset; no "to be determined" datasets).
3. **Planned architecture**: one of: (a) a CNN image classifier with transfer learning from a
   pretrained `torchvision.models` backbone, (b) an LSTM-based text-generation or time-series
   forecasting model, or (c) a simple VAE or GAN on an image dataset — built with PyTorch.
4. **Required evaluation**: state the evaluation appropriate to your chosen task (e.g., test
   accuracy and a confusion matrix for classification; reconstruction loss and qualitative
   samples for a VAE; discriminator/generator loss curves and sample quality for a GAN).
5. **Design trade-off to discuss**: state at least one design choice you plan to evaluate or
   compare (e.g., with vs. without transfer learning, optimizer A vs. B, LSTM vs. GRU, or VAE vs.
   GAN sample quality on the same dataset), holding everything else fixed between conditions where
   applicable.
6. **Team**: individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
use techniques covered in this course (CNNs, sequence models, attention/Transformer concepts, or
generative models, plus the practical training techniques from Weeks 5 and 13–14); techniques
beyond the syllabus require instructor pre-approval.

## Example Topics (for inspiration, not a closed list)
A CIFAR-10 or custom image classifier fine-tuned from a pretrained ResNet, comparing feature
extraction vs. full fine-tuning; an LSTM-based text generator or time-series forecaster comparing
LSTM vs. GRU; a simple VAE on Fashion-MNIST evaluated on reconstruction quality and latent-space
structure; a simple GAN on MNIST/Fashion-MNIST with a documented discussion of any mode collapse
or instability observed.
