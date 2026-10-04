# Week 2 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Universal Approximation and Expressivity

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the Universal Approximation Theorem for single-hidden-layer networks and explain its
   proof sketch in terms of localized bump functions. (*Understand*)
2. Explain precisely what the theorem does and does not guarantee. (*Understand, Analyze*)
3. Analyze depth-versus-width expressivity tradeoffs conceptually. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 1 landscape map; where approximation theory sits in it |
| 0:15–0:50 | The Universal Approximation Theorem | Statement for sigmoidal single-hidden-layer networks; proof sketch via sums of localized bumps |
| 0:50–1:00 | Break | — |
| 1:00–1:20 | What the theorem does *not* say | Existence, not learnability; no bound on required width; no claim about gradient descent finding the weights |
| 1:20–1:50 | Depth vs. width | Conceptual tradeoff: some functions need exponentially many units shallow but polynomially many once depth is allowed |
| 1:50–2:00 | Synthesis | Why approximation theory alone is an incomplete account of why deep learning works |

### Materials/Equipment
- Slides: bump-function construction diagram
- Live-coding environment (Jupyter) for the width-vs-approximation-error demo

### Formative Check (in-class)
Given a target 1D function with two "bumps," students sketch (on paper) the two sigmoidal units
needed to approximate one bump, and state how many total units a 3-bump target would need under
the construction shown.

### Link to Lab/Assessment
Lab 2: Empirically demonstrate universal approximation by fitting a single-hidden-layer network
to a 1D target function of increasing complexity as hidden width grows, and compare a
wide-shallow network to a narrow-deep network of similar parameter count (see
`lab-manuals/lab-02.md`).
