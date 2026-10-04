# Week 5 Lecture Plan — Introduction to Deep Learning
## Topic: Training Deep Networks at Scale — Augmentation, Schedules, Transfer Learning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply data augmentation pipelines with `torchvision.transforms` and explain their regularizing
   effect. (*Apply*)
2. Apply learning rate schedules and warmup, and explain why warmup stabilizes early training.
   (*Apply*)
3. Apply transfer learning by fine-tuning a pretrained `torchvision.models` backbone on a new task.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 4's CNN as the model that will now be trained "properly" |
| 0:15–0:35 | Data augmentation | Random crop/flip/color jitter; why augmentation regularizes |
| 0:35–1:00 | LR schedules & warmup | Step decay, cosine annealing; why warmup helps at the start of training |
| 1:00–1:10 | Break | — |
| 1:10–1:35 | Transfer learning concepts | Pretrained features as a starting point; feature extraction vs. fine-tuning |
| 1:35–2:00 | Fine-tuning live demo | Loading `resnet18(weights=...)`, replacing the head, freezing/unfreezing, fine-tuning |

### Materials/Equipment
- Live-coding environment, PyTorch, torchvision pretrained weights (downloaded in advance)

### Formative Check (in-class)
Given a small labeled image dataset, decide whether feature extraction (frozen backbone) or full
fine-tuning is more appropriate, and justify the choice in terms of dataset size.

### Link to Lab/Assessment
Lab 5: Add an augmentation pipeline and an LR schedule to Lab 4's CNN; fine-tune a pretrained
ResNet on the same dataset and compare against the from-scratch CNN.

### Assessment Note
**Assignment 1 is due** at the start of this week (per `course-plan.md` §6/7).
