# Week 7 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Neuro-Symbolic Integration I — Differentiable and Fuzzy Logic

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the t-norm/t-conorm relaxations of ∧/∨ and the standard negation relaxation.
   (*Understand, Apply*)
2. Implement product, Gödel, and Łukasiewicz t-norms and compare their gradient behavior.
   (*Apply, Analyze*)
3. Build a differentiable soft-logic loss and train it by gradient descent. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why Boolean step functions have no useful gradient |
| 0:15–0:45 | t-norms and t-conorms | Product, Gödel, Łukasiewicz; De Morgan duality |
| 0:45–1:10 | Soft implication & loss | Building a differentiable constraint-satisfaction loss |
| 1:10–1:40 | Gradient behavior | Why product is preferred over Gödel's near-zero gradient |
| 1:40–2:00 | Live coding | PyTorch soft-logic loss, gradient descent demo |

### Materials/Equipment
- Slides: "Differentiable & Fuzzy Logic for Neuro-Symbolic Learning"
- Jupyter/PyTorch for live coding

### Formative Check (in-class)
Compute the product-t-norm and Gödel-t-norm value of `0.9 ∧ 0.2`, then state which t-norm's
gradient w.r.t. the 0.9 input is nonzero and why that matters for learning.

### Link to Lab/Assessment
Lab 7: implement t-norms/t-conorms and a differentiable soft-logic loss in PyTorch (see
`lab-manuals/lab-07.md`). **Quiz 3** (Weeks 5–6 content) this week.
