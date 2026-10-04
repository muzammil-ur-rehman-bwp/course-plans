# Week 8 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: AI Safety and Alignment I; Midterm Review

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Explain specification gaming and reward hacking as a technical, Goodhart's-law-style
   phenomenon, with grounded examples. (*Analyze*)
2. Formalize the alignment problem and distinguish outer from inner alignment. (*Analyze,
   Evaluate*)
3. Consolidate Weeks 1–8 ahead of the midterm. (*Remember–Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Specification gaming | The CoastRunners case study and the broader documented pattern across RL benchmarks |
| 0:20–0:50 | The alignment problem, formalized | Specified objective vs. true intent; why "almost right" reward functions can still fail badly under optimization pressure |
| 0:50–1:20 | Outer vs. inner alignment | Definitions; the mesa-objective concept; why inner misalignment can be invisible during training |
| 1:20–1:55 | Midterm review | Structured recap across Weeks 1–8 (regret, bandits, contextual bandits, multi-agent RL, game theory, mechanism design, safety) |
| 1:55–2:00 | Logistics | Midterm format and coverage |

### Materials/Equipment
- Slides: "AI Safety I: Specification Gaming & the Alignment Problem"
- Midterm review handout (topic-by-topic checklist)

### Formative Check (in-class)
Given a short vignette of unwanted AI behavior, classify it as an outer-alignment failure, an
inner-alignment failure, or both, and justify the classification.

### Link to Lab/Assessment
Lab 8: build a toy gridworld with a misspecified reward and demonstrate specification gaming
(see `lab-manuals/lab-08.md`). Midterm exam in Week 9.
