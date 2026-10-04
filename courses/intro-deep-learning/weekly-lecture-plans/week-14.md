# Week 14 Lecture Plan — Introduction to Deep Learning
## Topic: Practical Deep Learning Workflows

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply an end-to-end transfer-learning workflow, from data loading to evaluation. (*Apply*)
2. Apply `state_dict`-based model saving/loading, and describe TorchScript/ONNX export
   conceptually. (*Apply*, *Understand*)
3. Evaluate a broken training run and diagnose the likely fault. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Synthesis | Assembling Weeks 2–13 components into one workflow |
| 0:15–0:45 | End-to-end workflow | Data loading → augmentation → pretrained backbone → fine-tuning → evaluation → iteration |
| 0:45–1:05 | Saving/loading/exporting | `state_dict` checkpoints; TorchScript tracing/scripting and ONNX export, conceptually |
| 1:05–1:15 | Break | — |
| 1:15–1:55 | Debugging a network that won't train | Overfit-one-batch test; data/label sanity checks; `model.train()`/`model.eval()` checks; gradient-norm inspection; LR sanity checks |
| 1:55–2:00 | Checklist recap | A cheapest-check-first debugging order |

### Materials/Equipment
- Live-coding environment, PyTorch
- Handout: a provided "broken" training script for the debugging exercise

### Formative Check (in-class)
Given a training run whose loss never decreases from the first step, name the single cheapest
diagnostic check to run first, and explain why it is cheaper than the alternatives.

### Link to Lab/Assessment
Lab 14: Assemble a full transfer-learning workflow end to end; save/reload the trained model; find
and fix the fault in a provided broken training script.
