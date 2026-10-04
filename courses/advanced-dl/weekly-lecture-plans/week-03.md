# Week 3 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Advanced Diffusion Techniques — Classifier-Free Guidance and Flow Matching

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Derive the classifier-free guidance formula from the implicit-classifier substitution.
   (*Analyze*)
2. Explain the diversity/fidelity tradeoff induced by the guidance scale $w$. (*Understand*)
3. Explain flow matching's velocity-regression objective and contrast its training cost with
   maximum-likelihood CNF training. (*Understand, Analyze*)
4. Implement classifier-free guidance and a minimal flow-matching sampler on 2-D toy data.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 2's score/SDE formulation recap; motivating conditional generation |
| 0:15–0:50 | Classifier-free guidance | Implicit-classifier substitution derived on the board; the guided-score formula |
| 0:50–1:10 | Guidance scale tradeoff | Why $w>1$ sharpens toward the condition at the cost of diversity |
| 1:10–1:40 | Flow matching | The velocity-field regression objective; CNFs; contrast with maximum-likelihood/trace-of-Jacobian training |
| 1:40–2:00 | Synthesis | Recap table: SDE sampler vs. flow-matching sampler vs. classifier-free guidance's role in either |

### Materials/Equipment
- Slides: "Advanced Diffusion Techniques"
- Whiteboard for the classifier-free-guidance derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Given $\nabla_x\log p_t(c\mid x) = \nabla_x\log p_t(x\mid c) - \nabla_x\log p_t(x)$, substitute
into the classical (classifier-guided) score formula and show it reduces to the classifier-free
guidance formula using only the conditional and unconditional scores.

### Link to Lab/Assessment
Lab 3: implement classifier-free guidance on the Week 2 toy score model, and a minimal
flow-matching training loop and ODE sampler, on 2-D synthetic data (see `lab-manuals/lab-03.md`).
Quiz 1 this week (Weeks 2–3 content).
