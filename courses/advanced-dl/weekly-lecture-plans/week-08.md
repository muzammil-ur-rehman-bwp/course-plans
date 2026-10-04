# Week 8 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Efficient Inference I — Quantization and Knowledge Distillation; Midterm Review

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Derive the affine post-training quantization mapping and explain the precision/accuracy
   tradeoff. (*Apply*)
2. Explain quantization-aware training's straight-through-estimator gradient conceptually.
   (*Understand*)
3. Derive the knowledge-distillation loss and explain the role of temperature scaling. (*Apply,
   Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Quantization motivation | Memory/throughput costs of FP32/FP16 at inference; the affine PTQ mapping derived |
| 0:25–0:45 | QAT | The straight-through estimator for the non-differentiable round op; accuracy recovery |
| 0:45–1:15 | Distillation | Teacher-student framework; the combined CE + temperature-scaled KL loss, derived; "dark knowledge" |
| 1:15–1:45 | Midterm review | Structured recap of Weeks 1–8's central derivations |
| 1:45–2:00 | Synthesis | Recap table: PTQ vs. QAT vs. distillation — what each trades off |

### Materials/Equipment
- Slides: "Efficient Inference I: Quantization and Distillation"
- Whiteboard for the affine-quantization and distillation-loss derivations
- Midterm review handout (Weeks 1–8 checklist)

### Formative Check (in-class)
Given a weight tensor's min/max range and a target bit-width $b$, compute the affine scale $s$ and
zero-point $z$, quantize and dequantize a sample value, and state the resulting quantization
error.

### Link to Lab/Assessment
Lab 8: implement PTQ and a straight-through-estimator QAT training loop, and the knowledge-
distillation loss (see `lab-manuals/lab-08.md`). Capstone problem-statement check-in this week.
