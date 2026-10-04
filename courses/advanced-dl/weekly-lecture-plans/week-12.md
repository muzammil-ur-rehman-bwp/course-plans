# Week 12 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Current Frontier Generative-Modeling Survey (Grounded, Fast-Moving)

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the consistency-model mechanism's training objective and explain why it collapses
   many-step sampling to one or few steps. (*Understand, Evaluate*)
2. State progressive/step-distillation as a direct application of Week 8's teacher-student
   framing to a sampling trajectory. (*Analyze*)
3. Critically evaluate a current fast-sampler technique's reported quality/latency tradeoff,
   flagging what should be read with caution. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Framing | Explicit "fast-moving area" flag; how to read this week's survey critically |
| 0:15–0:45 | Consistency-model-style ideas | Mapping any trajectory point to the same clean endpoint; why this enables few-step sampling |
| 0:45–1:15 | Progressive/step distillation | Teacher = many-step sampler, student = few-step sampler; reusing Week 8's distillation framing |
| 1:15–1:45 | The current landscape | Competing quality/latency tradeoffs; the moving Pareto frontier |
| 1:45–2:00 | Synthesis | Recap + explicit caveats about dating quickly |

### Materials/Equipment
- Slides: "Frontier Generative-Modeling Survey"
- Instructor-curated current reading (refreshed each offering)

### Formative Check (in-class)
State, in one sentence each, what plays the role of "teacher" and "student" in a progressive/
step-distillation fast sampler, and why this is a direct reuse of Week 8's distillation framing
rather than a new concept.

### Link to Lab/Assessment
Lab 12 (critical-writing, optional light code): critique a current fast-sampler paper's reported
tradeoff, flagging what should be read with appropriate caution (see `lab-manuals/lab-12.md`).
