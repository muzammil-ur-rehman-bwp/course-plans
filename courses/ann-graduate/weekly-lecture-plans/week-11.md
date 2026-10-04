# Week 11 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Expressivity and Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain, as an optimization-theory argument (not an architecture lecture), why skip
   connections ease optimization in very deep networks. (*Analyze*)
2. Explain attention mechanisms from an expressivity viewpoint, briefly and without architectural
   depth. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & scope reminder | This week is theory-angle only; architecture depth is the Deep Learning course's territory |
| 0:15–0:50 | Skip connections, the optimization argument | A skip connection lets a layer represent identity trivially; why this keeps gradients from vanishing through many stacked layers and smooths the landscape near identity init |
| 0:50–1:00 | Break | — |
| 1:00–1:30 | Gradient flow, plain vs. skip | Comparing gradient magnitude through depth with and without skip paths, conceptually and then empirically |
| 1:30–2:00 | Attention, the expressivity argument | A weighted combination over all positions gives a more direct, less depth-bottlenecked information path than a purely sequential/local layer — brief, conceptual framing only |

### Materials/Equipment
- Slides: gradient-flow diagrams, plain vs. skip-connected depth
- Live-coding environment (Jupyter) for the gradient-flow comparison and a toy attention-weight
  visualization

### Formative Check (in-class)
Students explain, in one or two sentences, why a layer initialized so that its residual branch
starts near zero makes the whole block behave like (approximately) an identity map at $t=0$, and
why that matters for very deep networks.

### Link to Lab/Assessment
Lab 11: Compare gradient-flow magnitude through a deep plain feedforward network versus an
equivalent network with skip connections, both near identity initialization, and build a toy
attention-weight visualization illustrating the expressivity argument (see
`lab-manuals/lab-11.md`).
