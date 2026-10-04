# Week 10 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Neural Architecture Search

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Decompose a given NAS method description into search space, search strategy, and performance
   estimation. (*Analyze*)
2. Explain RL-based and evolutionary search strategies conceptually. (*Understand*)
3. Explain differentiable NAS's continuous relaxation conceptually. (*Understand*)
4. Evaluate NAS's own search cost as a practical constraint and connect it to weight-sharing/
   differentiable-relaxation developments. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | The NAS problem | Search space / search strategy / performance estimation, decomposed |
| 0:20–0:45 | RL-based search | Controller-samples-architecture-as-policy-gradient framing (REINFORCE referenced, not re-derived) |
| 0:45–1:10 | Evolutionary search | Population, mutation, selection |
| 1:10–1:40 | Differentiable NAS | The softmax-relaxation trick (DARTS-style), conceptually |
| 1:40–2:00 | The cost problem | Why search cost itself drove every later NAS development |

### Materials/Equipment
- Slides: "Neural Architecture Search"
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Given a short description of a NAS paper's method, identify its search space, search strategy,
and performance-estimation strategy in one sentence each.

### Link to Lab/Assessment
Lab 10: implement a small evolutionary search over a toy architecture space, and a minimal
differentiable-NAS-style operation relaxation (see `lab-manuals/lab-10.md`).
