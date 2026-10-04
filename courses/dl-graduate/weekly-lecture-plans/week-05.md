# Week 5 Lecture Plan — Deep Learning (Graduate)
## Topic: Self-Supervised and Contrastive Representation Learning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the pretext-task idea and why it enables learning from unlabeled data. (*Understand*)
2. Derive the InfoNCE contrastive loss from the positive-vs-negatives classification framing.
   (*Analyze*)
3. Implement a SimCLR-style contrastive training step in PyTorch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Transfer learning from the undergraduate course; today: learning representations without labels at all |
| 0:15–0:35 | The pretext-task idea | Constructing a supervised-looking task from unlabeled data |
| 0:35–1:05 | Contrastive learning & InfoNCE | Positive pairs, negatives, the InfoNCE loss derivation, temperature |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | SimCLR-style framing | Two augmented views, shared encoder, projection head, batch-as-negatives |
| 1:40–2:00 | Evaluating representations | Linear probing, conceptually; why it isolates representation quality |

### Materials/Equipment
- Live-coding environment, PyTorch, torchvision augmentations
- Slide diagram: SimCLR two-view pipeline with projection head

### Formative Check (in-class)
For a batch of 4 images (2 augmented pairs), students write out the InfoNCE loss's numerator and
denominator terms for one anchor by hand, identifying the positive and the 3 negatives.

### Link to Lab/Assessment
Lab 5: Implementing the InfoNCE loss and a minimal SimCLR-style training step on a small image
dataset (see `lab-manuals/lab-05.md`).
