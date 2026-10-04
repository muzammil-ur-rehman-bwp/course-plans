# Week 4 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Mixture-of-Experts and Sparse Architectures

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Write the MoE layer's gating/routing equation and its top-$k$ sparse-dispatch form. (*Apply*)
2. Analyze why sparse activation decouples total parameter count from per-example compute.
   (*Analyze*)
3. Explain the load-balancing problem and at least one practical mitigation. (*Understand,
   Analyze*)
4. Implement a top-$k$ MoE layer and a load-balancing auxiliary loss in PyTorch. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why a single dense FFN limits how parameters scale relative to compute |
| 0:15–0:45 | The MoE layer | Router, softmax gate, top-$k$ selection, dispatch; board-worked FLOP count |
| 0:45–1:10 | The decoupling argument | Parameters scale with $N$, compute scales with $k$ — why this matters at scale |
| 1:10–1:40 | Load balancing | Router collapse; the auxiliary coefficient-of-variation loss; noisy gating |
| 1:40–2:00 | Synthesis | MoE's placement in a Transformer block; recap table |

### Materials/Equipment
- Slides: "Mixture-of-Experts and Sparse Architectures"
- Whiteboard for the FLOP-count derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
For $N=8$ experts, $k=2$, and a per-expert FFN cost of $F$ FLOPs, compute the MoE layer's
per-token FLOP cost and its total parameter count, and compare both to a dense FFN with the same
total parameter count as all 8 experts combined.

### Link to Lab/Assessment
Lab 4: implement a top-$k$ MoE layer from scratch and a load-balancing auxiliary loss, and compare
FLOPs/parameters against a matched dense baseline (see `lab-manuals/lab-04.md`).
Assignment 1 assigned this week (Weeks 2–4).
