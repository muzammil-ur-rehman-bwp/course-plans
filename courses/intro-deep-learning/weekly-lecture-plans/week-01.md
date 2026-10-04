# Week 1 Lecture Plan — Introduction to Deep Learning
## Topic: The Deep Learning Landscape and the PyTorch Framework Tour

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the prerequisite pipeline (perceptron → MLP → backpropagation → optimizers) as assumed
   knowledge for this course. (*Remember*)
2. Explain why depth and learned hierarchical representations matter in modern deep learning.
   (*Understand*)
3. Apply PyTorch tensors, autograd, `nn.Module`, and the training loop to reproduce a result
   already understood from the prerequisite course. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:10 | Course framing | How this course extends *Introduction to Artificial Neural Networks*; syllabus walkthrough |
| 0:10–0:25 | Prerequisite recap (stated, not re-derived) | One slide each: perceptron, MLP, backpropagation, SGD/Adam, L2/dropout |
| 0:25–0:50 | Why depth matters | Hierarchical features, representation learning, landscape tour (vision/NLP/speech/generative) |
| 0:50–1:00 | Break | — |
| 1:00–1:20 | PyTorch tensors & autograd | Tensor creation, `requires_grad`, `.backward()`, comparing against a known hand-derived gradient |
| 1:20–1:45 | `nn.Module` and the training loop | Defining a model class; `zero_grad → forward → loss → backward → step` |
| 1:45–2:00 | Live demo | Training a tiny MLP (from the prerequisite course) in PyTorch in under 15 lines |

### Materials/Equipment
- Live-coding environment, PyTorch (Colab or local with GPU)
- Syllabus and course-plan handout

### Formative Check (in-class)
Given a two-tensor expression with `requires_grad=True`, predict the `.grad` values by hand using
chain-rule knowledge from the prerequisite course, then verify with `.backward()`.

### Link to Lab/Assessment
Lab 1: Tensor/autograd tour; reimplementing a known small MLP's forward and backward pass with
PyTorch and confirming it matches prior NumPy-based results.
