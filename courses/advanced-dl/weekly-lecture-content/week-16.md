# Week 16 — Lecture Content: Capstone Presentations and Course Review

## 1. The Defense Format
Each student presents their final research proposal in a qualifying-exam/thesis-proposal-defense
format (see `presentations/capstone-presentation-template.md` for the required slide structure):
problem statement, related work, proposed approach, feasibility argument or preliminary results,
anticipated risks, and committee-style Q&A. This is evaluated as a thesis-proposal committee would
evaluate a candidate's proposal before the dissertation work has happened — see
`assignments/capstone-rubric.md` for the full grading breakdown.

## 2. Course Map Recap
```
Week 1       Course overview / scoping vs. sibling postgraduate courses
Weeks 2–3    SDE score-based diffusion → classifier-free guidance / flow matching
Week 4       Mixture-of-Experts and sparse architectures
Week 5       In-context learning mechanics (induction heads vs. implicit gradient descent)
Weeks 6–7    Bradley-Terry / RLHF pipeline → PPO / Direct Preference Optimization
Weeks 8–9    Quantization / distillation → (midterm) → speculative decoding
Week 10      Neural architecture search
Week 11      Scaling laws — systems/engineering perspective
Week 12      Frontier generative-modeling survey (fast samplers, distillation)
Week 13      Research methods for applied DL research
Week 14      Current open problems survey
Weeks 15–16  Research proposal work session → capstone defenses
```
Each topic extended a specific piece of the graduate *Deep Learning* course (SDE diffusion
extends DDPM; MoE extends the Transformer FFN sublayer; RLHF/DPO extends policy gradients/
actor-critic; NAS and systems-scaling extend large-scale-training basics) — the throughline
Week 1 opened with.

## 3. Connections to the Sibling Postgraduate Courses
- *Advanced Artificial Neural Network*: the theoretical "why do power laws hold" question this
  course's Week 11 deliberately did not re-ask; NTK/mean-field/grokking/PAC-Bayes material a
  capstone extending Week 4's MoE or Week 10's NAS into a theoretical generalization question
  might eventually need.
- *Advanced Artificial Intelligence*: the alignment-as-research-agenda framing (specification
  gaming, outer/inner alignment, scalable oversight) this course's Weeks 6–7 deliberately did not
  re-ask; relevant to a capstone that wants to examine *whether* RLHF/DPO is philosophically
  adequate as an alignment approach, not just how to implement it.
- *Advanced Machine Learning*: statistical learning theory (minimax bounds, causal inference,
  robust statistics) with no expected overlap, but potentially relevant background for a capstone
  wanting a more rigorous statistical treatment of, e.g., reward-model uncertainty.

## 4. Where This Leads
This course's frontier-engineering foundation is a direct on-ramp to current deep learning
research venues (NeurIPS, ICML, ICLR) and to thesis-level research in any of this course's eight
topic areas. A strong capstone proposal here is, by design, close in shape to an actual PhD
qualifying-exam proposal or an early thesis-proposal draft — the same four-part structure
(problem statement, related-work survey, proposed approach, feasibility argument) is not a course
artifact invented for this class, but the structure real thesis committees use.

## 5. In-Class Activity
Attend and constructively question classmates' capstone defenses using the same four review
questions from Week 15. After all defenses, as a class, revisit the Week 1 scoping table and
confirm, now having completed the course, that each week's placement (here vs. a sibling course)
still makes sense in light of the semester's actual content.
