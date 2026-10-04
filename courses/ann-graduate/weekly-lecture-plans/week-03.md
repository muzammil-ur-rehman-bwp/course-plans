# Week 3 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Automatic Differentiation in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Represent a computation as a computational graph of elementary operations. (*Understand*)
2. Differentiate forward-mode from reverse-mode automatic differentiation and state when each is
   more efficient. (*Analyze*)
3. Formalize backpropagation as a special case of reverse-mode automatic differentiation, and
   implement a minimal reverse-mode autodiff engine. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Approximation theory tells us networks *can* represent a function; now, how are their gradients actually computed at scale? |
| 0:15–0:40 | Computational graphs | Nodes as operations, edges as data dependencies; a worked small example graph |
| 0:40–1:10 | Forward-mode AD | Propagating tangents $\dot v = \partial v/\partial x$ forward; efficient for few inputs, many outputs |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Reverse-mode AD | Propagating adjoints $\bar v = \partial L/\partial v$ backward from a scalar output; efficient for many inputs, one output |
| 1:45–2:00 | Backprop = reverse-mode AD | Mapping the layer-by-layer backprop students already know onto the general reverse-mode algorithm |

### Materials/Equipment
- Slides: a small computational graph with forward/reverse sweeps annotated
- Live-coding environment (Jupyter) for building a scalar autodiff engine

### Formative Check (in-class)
Given a 3-operation computational graph ($f = (x\cdot y) + \sin(x)$), students compute both the
forward-mode tangent and reverse-mode adjoint for $\partial f/\partial x$ by hand and confirm they
agree.

### Link to Lab/Assessment
Lab 3: Implement a minimal scalar-valued reverse-mode automatic differentiation engine from
scratch in Python, and verify it reproduces hand-derived gradients from the prerequisite course
(see `lab-manuals/lab-03.md`).
