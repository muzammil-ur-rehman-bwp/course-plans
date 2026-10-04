# Week 12 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: The Neural Tangent Kernel

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the Neural Tangent Kernel (NTK) framework's central claim about infinite-width
   networks trained with gradient descent. (*Understand, Analyze*)
2. Evaluate what the NTK view reveals about why wide networks train easily, and its limits.
   (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Depth/skip-connection theory explained *trainability*; NTK offers a different, width-based account |
| 0:15–0:50 | The NTK claim | As width $\to \infty$, a network's training dynamics under gradient descent become equivalent to kernel regression against a fixed kernel (the NTK), determined at initialization and (in the infinite-width limit) unchanging during training |
| 0:50–1:00 | Break | — |
| 1:00–1:30 | What this explains | Why sufficiently wide networks can reach zero training loss via convex-like dynamics in function space; a tractable theoretical account of easy trainability |
| 1:30–2:00 | The limits | The kernel is fixed — it does not capture feature learning; finite, realistic-width networks are widely believed to rely on feature learning for their strongest generalization, so NTK is a partial, not complete, theory |

### Materials/Equipment
- Slides: NTK definition and infinite-width-limit argument (conceptual, not the full derivation)
- Live-coding environment (Jupyter) for the small-scale NTK computation

### Formative Check (in-class)
Students state, in their own words, the one-sentence distinction between "a wide network trains
like kernel regression against a fixed kernel" and "a wide network learns useful features," and
why only the former is what NTK theory establishes.

### Link to Lab/Assessment
Lab 12: Numerically compute an NTK-flavored kernel for a simple one-hidden-layer network at
initialization, and compare kernel-regression predictions against that kernel to predictions from
actually training the network with gradient descent on the same small dataset (see
`lab-manuals/lab-12.md`).
