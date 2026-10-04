# Week 11 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Modern Generalization Bounds — PAC-Bayes

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why classical VC/Rademacher-style bounds are often numerically vacuous for deep
   networks. (*Understand, Analyze*)
2. State the structure of a PAC-Bayes generalization bound (prior, posterior, KL term, confidence
   term). (*Understand, Apply*)
3. Evaluate why a PAC-Bayes bound can be non-vacuous where a classical bound is not, and connect
   this to Week 6's sharpness material. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Recap | Classical VC/Rademacher bounds (graduate course); why they scale with worst-case hypothesis-class complexity |
| 0:20–0:40 | Why they go vacuous | Deep networks' raw parameter-count-driven capacity measures swamp the bound |
| 0:40–1:20 | PAC-Bayes, derived structurally | Posterior vs. prior; the KL-divergence term; the confidence term; the bound's core trade-off |
| 1:20–1:50 | Connection to sharpness | A concentrated posterior near a wide/flat region the prior favors tightens the bound |
| 1:50–2:00 | Synthesis | What "non-vacuous" means in practice, and its limits |

### Materials/Equipment
- Slides: "PAC-Bayes: A Tighter Generalization Framework"
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Explain, in one or two sentences, why a posterior that places almost all its mass on a single
point (near-deterministic) makes the KL term in a PAC-Bayes bound behave very differently from a
posterior that spreads mass over a wide, flat region favored by the prior.

### Link to Lab/Assessment
Lab 11: compute a PAC-Bayes-style bound for a small trained network under a Gaussian-perturbation
posterior and compare it to a classical VC-style bound's value on the same network (see
`lab-manuals/lab-11.md`).
