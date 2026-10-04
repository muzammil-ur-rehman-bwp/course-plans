# Week 1 Lecture Plan — Deep Learning (Graduate)
## Topic: Graduate Deep Learning Overview

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the assumed undergraduate DL survey (CNN/RNN/attention/Transformer-survey/VAE/GAN) and
   the assumed tabular-MDP/RL foundation as prior knowledge for this course. (*Remember*)
2. Explain how this course differs from *Introduction to Deep Learning* and from the sibling
   graduate courses (*Artificial Neural Network*, *Artificial Intelligence*). (*Understand*)
3. Map a given topic/paper title onto the week of this course's syllabus it belongs to.
   (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Course framing | How this course extends *Introduction to Deep Learning* and builds on *Artificial Intelligence*'s tabular MDP/RL; syllabus walkthrough |
| 0:15–0:40 | Prerequisite recap (stated, not re-taught) | One slide each: CNN basics, LSTM/GRU, attention, Transformer survey, VAE/GAN basics, tabular Bellman/Q-learning |
| 0:40–1:00 | Scope boundaries | What this course does NOT re-teach (undergraduate CNN/RNN/attention; ANN-graduate's theory; AI-graduate's MDP formalism) and why |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | The architecture-and-technique landscape | Advanced CNNs, Transformer-from-scratch, self-supervised learning, diffusion, GNNs, deep RL, large-scale training, multimodal survey — one slide each as a map |
| 1:40–2:00 | Capstone preview | Capstone structure (literature review, experiment, paper, presentation); topic-selection timeline (Weeks 7–8) |

### Materials/Equipment
- Syllabus and course-plan handout
- Colab notebook for environment/GPU-runtime check

### Formative Check (in-class)
Given five paper titles/topic phrases, students sort each into the correct week/module of this
course's syllabus and justify the placement in one sentence.

### Link to Lab/Assessment
Lab 1: Environment setup (PyTorch/torchvision/`torch_geometric`, GPU runtime check) and a
prerequisite-fluency warm-up exercise (reimplementing a known small CNN forward pass from the
undergraduate course in PyTorch, confirming output shapes) (see `lab-manuals/lab-01.md`).
