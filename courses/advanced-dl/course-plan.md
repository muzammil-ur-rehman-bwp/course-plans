# Course Plan: Advanced Deep Learning (Post Graduate)

## 1. Course Information

| Field | Detail |
|---|---|
| Course Title | Advanced Deep Learning |
| Level | Post Graduate (PhD-track: PhD coursework, or an MS student heading toward a thesis) |
| Credit Hours | 3 (2 hrs lecture + 1 research seminar/lab session of 3 hrs/week) |
| Prerequisites | **Deep Learning (Graduate), or equivalent.** Students must already be fluent with: the Transformer architecture derived and implemented from scratch (scaled dot-product attention, multi-head attention, positional encoding, encoder-decoder composition); advanced CNN architectures (ResNet, DenseNet, efficiency-focused designs); self-supervised and contrastive representation learning (InfoNCE); diffusion models at the discrete-time DDPM level (forward noising process, reverse denoising process, the simplified training objective, sampling); graph neural networks (message passing, GCN, GraphSAGE, GAT); deep reinforcement learning (DQN, REINFORCE, actor-critic); and large-scale training basics (data/model parallelism, mixed precision, gradient checkpointing, LoRA). This course does **not** re-teach any of that; every one of those results is assumed firm, working background and is extended, not repeated. |
| Programming Language | Python 3.x |
| Core Libraries | PyTorch (primary framework throughout); no new heavyweight dependency is required beyond what the graduate course already used |
| Duration | 16 teaching weeks (1 semester) + 1 exam week |
| Delivery Mode | Lecture + research seminar/lab (concept lecture followed by an implementation, critical-writing, or proposal-development session, depending on the week; GPU access assumed/recommended) |

## 2. Course Description

This is the most advanced course in the deep learning sequence (undergraduate *Introduction to
Deep Learning* → graduate *Deep Learning* → this postgraduate course), and it is a PhD-track,
frontier-research-and-engineering treatment of deep learning. It assumes the graduate course's
architectures and techniques — the Transformer from scratch, advanced CNNs, self-supervised and
contrastive learning, discrete-time DDPM diffusion, graph neural networks, deep RL, and
large-scale training basics — as settled background, and spends every week pushing past them into
territory the graduate course explicitly does not cover: the stochastic differential equation
(SDE) formulation of score-based generative models (of which discrete-time DDPM is a
discretization); advanced diffusion techniques (classifier-free guidance, flow matching); Mixture-
of-Experts and sparse architectures; the mechanics of in-context learning; concrete preference-
based alignment engineering (the Bradley-Terry model, the RLHF reward-modeling/PPO pipeline, and
Direct Preference Optimization derived as its simplification); efficient inference (quantization,
distillation, speculative decoding); neural architecture search; and scaling laws examined from a
systems/engineering perspective (compute-optimal training in practice). The course closes with a
genuine PhD-qualifying-exam-style research-proposal capstone.

**This course is deliberately scoped to avoid duplicating three sibling postgraduate courses.**
It does **not** own the theoretical "why do scaling laws hold" question, NTK/mean-field theory,
implicit bias, grokking, statistical-physics approaches to loss landscapes, PAC-Bayes bounds, or
mode connectivity — that is *Advanced Artificial Neural Network*'s territory; Week 11 here studies
scaling laws from a **systems/engineering** angle (practical compute-optimal allocation, real
training-at-scale engineering challenges) and explicitly points to, rather than repeats, that
course's theoretical treatment. It does **not** own regret/bandit theory, multi-agent RL, game
theory and mechanism design, or AI safety and alignment framed as a research-agenda/philosophical
topic — that is *Advanced Artificial Intelligence*'s territory; Weeks 6–7 here cover the
**concrete engineering techniques** (RLHF's pipeline, PPO, DPO) that make preference-based
alignment work in practice, not alignment-as-philosophy. It does **not** own minimax bounds,
high-dimensional statistics, full-information online convex optimization, nonparametric Bayesian
methods, causal inference, algorithmic fairness, or robust statistics — that is *Advanced Machine
Learning*'s territory, with no expected overlap with this course's content.

## 3. Goals

- Build directly on the graduate course's Transformer-from-scratch, advanced-CNN,
  self-supervised-learning, DDPM-diffusion, GNN, deep-RL, and large-scale-training foundation,
  without re-teaching any of it, to study genuinely frontier architectures and techniques.
- Derive the stochastic differential equation (SDE) formulation of score-based generative
  modeling — the forward SDE as continuous-time noising, the reverse-time SDE for sampling, and
  the score-matching objective — and show how discrete-time DDPM is recovered as a discretization.
- Derive classifier-free guidance's sampling formula and understand flow matching/continuous
  normalizing flows as an alternative continuous-time generative framework, at a conceptual level.
- Derive the Mixture-of-Experts layer's gating/routing formulation and understand why sparse
  activation decouples parameter count from per-example compute, including load-balancing
  challenges.
- Critically distinguish well-supported empirical findings (e.g., induction heads) from more
  speculative mechanistic theory (e.g., the implicit-gradient-descent analogy) when studying the
  mechanics of in-context learning.
- Derive the Bradley-Terry model for pairwise preference data and the RLHF pipeline structure, and
  derive Direct Preference Optimization as a reparameterization of the RLHF objective that avoids
  training an explicit reward model, building explicitly on the graduate course's policy-
  gradient/actor-critic foundation without re-deriving it.
- Understand efficient inference techniques — quantization (including the precision/accuracy
  tradeoff), knowledge distillation, and speculative decoding's correctness argument — and apply
  them in PyTorch.
- Understand neural architecture search as a search-space/search-strategy/performance-estimation
  problem, including RL-based, evolutionary, and differentiable approaches, and critically weigh
  NAS's own compute cost as a practical constraint.
- Evaluate scaling laws from a systems/engineering perspective — compute-optimal allocation
  between model size and data in practice, and real engineering challenges of training at scale —
  explicitly distinct from a theoretical "why" treatment.
- Critically survey current frontier generative-modeling research (fast samplers, distillation of
  diffusion models) and 2–3 currently open engineering/research problems, without hype, flagged
  explicitly as a fast-moving area.
- Design, write, and defend an original PhD-qualifying-exam-style research proposal: a problem
  statement, a related-work survey of five or more papers, a proposed novel approach, and either
  preliminary results or a rigorous feasibility argument.

## 4. Course Learning Outcomes (CLOs) — Mapped to Bloom's Taxonomy

| CLO | Statement | Bloom's Level(s) |
|---|---|---|
| CLO1 | Recall the graduate-DL architectures/techniques this course assumes, and map the postgraduate frontier-DL landscape this course covers, scoped explicitly against the sibling postgraduate courses. | Remember, Understand |
| CLO2 | Derive the forward/reverse SDE formulation of score-based generative models and the score-matching objective, and show discrete-time DDPM as its discretization; implement a toy SDE sampler. | Apply, Analyze |
| CLO3 | Derive classifier-free guidance's sampling formula and explain flow matching/continuous normalizing flows conceptually. | Apply, Analyze |
| CLO4 | Derive the Mixture-of-Experts gating/routing formulation and analyze why sparse activation decouples parameter count from per-example compute, including load-balancing. | Apply, Analyze |
| CLO5 | Critically evaluate current mechanistic hypotheses for in-context learning, distinguishing well-supported empirical findings from speculative theory. | Analyze, Evaluate |
| CLO6 | Derive the Bradley-Terry preference model and the RLHF pipeline, and derive Direct Preference Optimization as a reparameterization of the RLHF objective, building on (not re-deriving) policy-gradient/actor-critic foundations. | Apply, Analyze |
| CLO7 | Apply and evaluate efficient-inference techniques — quantization, knowledge distillation, and speculative decoding — including their correctness and accuracy/efficiency tradeoffs. | Apply, Analyze, Evaluate |
| CLO8 | Analyze the neural architecture search problem (search space, strategy, performance estimation) across RL-based, evolutionary, and differentiable approaches, and evaluate NAS's own compute cost as a practical constraint. | Analyze, Evaluate |
| CLO9 | Evaluate compute-optimal scaling and large-scale training engineering practice from a systems perspective, explicitly distinct from a theoretical account of why power laws hold. | Evaluate |
| CLO10 | Critically survey current frontier generative-modeling research and open engineering problems, and design, write, and defend an original PhD-qualifying-exam-style research proposal: a problem statement, a 5+ paper related-work survey, a proposed novel approach, and a feasibility argument or preliminary results. | Analyze, Evaluate, Create |

### Bloom's Taxonomy progression across the semester

| Phase | Weeks | Dominant Bloom's Levels | Focus |
|---|---|---|---|
| Postgraduate Orientation & Advanced Generative Modeling | 1–3 | Remember, Understand, Apply, Analyze | Frontier-DL landscape; SDE score-based diffusion; classifier-free guidance and flow matching |
| Sparse Architectures, In-Context Learning & Alignment Foundations | 4–6 | Apply, Analyze | Mixture-of-Experts; in-context learning mechanics; Bradley-Terry/RLHF pipeline |
| Alignment Engineering & Efficient Inference | 7–9 | Apply, Analyze, Evaluate | PPO/DPO; quantization/distillation; midterm; speculative decoding |
| Systems, Scaling, Research Methods & Capstone | 10–16 | Evaluate, Create | NAS; systems-view scaling laws; frontier survey; research methods; open problems; proposal work; capstone defense |

## 5. Weekly Topic Overview (16 Weeks)

| Week | Topic | Bloom's Focus |
|---|---|---|
| 1 | Course overview: frontier-DL landscape; rapid review of assumed graduate-DL foundations; explicit scope boundaries vs. the sibling postgraduate courses | Remember, Understand |
| 2 | Score-based generative models: the SDE formulation of diffusion (forward/reverse-time SDEs), score matching, connection to discrete-time DDPM | Apply, Analyze |
| 3 | Advanced diffusion techniques: classifier-free guidance, derived; flow matching/continuous normalizing flows (conceptual) | Apply, Analyze |
| 4 | Mixture-of-Experts and sparse architectures: gating/routing, sparse activation, load-balancing | Apply, Analyze |
| 5 | In-context learning mechanics: the empirical phenomenon; induction heads; the implicit-gradient-descent analogy, critically weighed | Analyze, Evaluate |
| 6 | Preference-based alignment I: the Bradley-Terry model; the RLHF pipeline structure (SFT → reward model → RL fine-tuning) | Apply, Analyze |
| 7 | Preference-based alignment II: PPO for RLHF (conceptual, built on policy gradients); Direct Preference Optimization derived | Apply, Analyze |
| 8 | Efficient inference I: quantization; knowledge distillation; midterm review | Apply, Analyze |
| 9 | **Midterm Exam** + Efficient inference II: speculative decoding; KV-cache management (conceptual) | Apply, Analyze |
| 10 | Neural architecture search: search space/strategy/performance estimation; RL-based and evolutionary search; differentiable NAS (conceptual) | Apply, Analyze |
| 11 | Scaling laws from a systems/engineering perspective: compute-optimal training in practice; engineering challenges at scale | Evaluate |
| 12 | Current frontier generative-modeling survey (grounded, flagged as fast-moving): fast samplers and diffusion distillation | Evaluate |
| 13 | Research methods for applied DL research: critiquing systems-and-methods papers; benchmark culture and reproducibility; capstone work time | Analyze, Evaluate |
| 14 | Current open problems survey (grounded, flagged as fast-moving) | Evaluate |
| 15 | Research proposal work session: drafting, refining, peer feedback | Apply, Create |
| 16 | Capstone research proposal presentations + course review | Evaluate, Create |
| 17 | Final Exam Week | — |

## 6. Assessment Plan

| Component | Weight | Notes |
|---|---|---|
| Lab Work (weekly) | 10% | Graded notebooks/exercises, Labs 1–15 |
| Assignments (2 problem sets) | 10% | Tied to Weeks 4 and 9 |
| Quizzes (6, best 5 counted) | 10% | Short, in-class/online, 15 min each |
| Paper Critique & Presentation | 10% | Week 13 research-skills assignment; written critique + in-class presentation of a current DL systems/methods paper |
| Midterm Exam | 10% | Week 9, qualifying-exam style, covers Weeks 1–8 |
| Research Proposal Capstone | 40% | Problem statement, 5+ paper related-work survey, proposed novel approach, feasibility argument/preliminary results, written proposal, and oral defense |
| Final Exam | 10% | Comprehensive, emphasis on Weeks 9–14 |

**Rationale for the postgraduate weighting.** As in the sibling postgraduate courses, weight
shifts decisively toward independent research: exams (Midterm + Final) total only 20%, and weekly
Labs drop to 10% because postgraduate lab sessions increasingly fold into proposal-development
work by the back half of the semester. The Research Proposal Capstone, at 40%, is by far the
largest single component and is judged like a thesis-proposal defense: a sound problem statement,
a genuine survey of the related work, a defensible proposed approach, and an honest feasibility
argument matter more than exam recall or completing a fixed assignment. The components total
exactly 100% (10 + 10 + 10 + 10 + 10 + 40 + 10 = 100).

## 7. Grading Policy

Standard letter grading per institutional policy (e.g., A ≥ 85, B ≥ 70, C ≥ 55, D ≥ 40, F < 40;
adjust to institution). Late submissions: −10% per day up to 3 days, then not accepted unless
documented emergency. Capstone milestones (problem-statement check-in Week 8, draft proposal Week
15, final proposal + defense Week 16) have fixed deadlines because of the downstream defense
schedule; late capstone milestones are handled case-by-case with the instructor.

## 8. Tools & Software

- Python 3.10+, pip/conda
- Jupyter Notebook / Google Colab (GPU access assumed/recommended throughout)
- PyTorch (primary framework for the entire course; no new heavyweight dependency beyond what the
  graduate course already required)
- Matplotlib for visualization throughout
- Git/GitHub for lab, assignment, and capstone submission

## 9. Reference Reading

- Song et al., on the stochastic differential equation formulation of score-based generative
  modeling (forward/reverse-time SDEs and score matching) — primary source for Week 2.
- Ho et al., on Denoising Diffusion Probabilistic Models — already a reference in the graduate
  course at the discrete-time level; revisited in Week 2 as the SDE formulation's discretization.
- Current, well-established work on classifier-free guidance and on flow matching as a
  continuous-time generative framework, as covered in lecture — used for Week 3.
- Shazeer et al., on Mixture-of-Experts / outrageously large neural networks — primary source for
  Week 4.
- Brown et al., on GPT-3's few-shot in-context learning — primary source for Week 5's empirical
  phenomenon.
- Current mechanistic-interpretability research on induction heads, and current (more
  speculative) work relating in-context learning to implicit gradient descent, as covered in
  lecture and explicitly distinguished by evidentiary strength — used for Week 5.
- Ouyang et al., on InstructGPT and the RLHF pipeline (supervised fine-tuning, reward modeling,
  PPO-based RL fine-tuning) — primary source for Weeks 6–7.
- Rafailov et al., on Direct Preference Optimization — primary source for Week 7's DPO derivation.
- Hinton et al., on knowledge distillation — primary source for Week 8.
- Current, well-established work on post-training quantization and quantization-aware training,
  and on speculative decoding, as covered in lecture — used for Weeks 8–9.
- Current, well-established work on neural architecture search (RL-based, evolutionary, and
  differentiable search strategies), as covered in lecture — used for Week 10.
- Hoffmann et al.'s compute-optimal scaling refinement, already introduced from a theoretical
  angle in the sibling *Advanced Artificial Neural Network* course — revisited here strictly from
  a systems/engineering, compute-allocation-in-practice angle — used for Week 11, alongside the
  graduate course's own Week 12 large-scale-training material.
- Current, well-established work on fast diffusion samplers and consistency-model-style
  distillation, as covered in lecture — used for Week 12 (explicitly flagged as a fast-moving
  area).
- Official PyTorch documentation.

Where a specific year, venue, or identifier is not given above, students are expected to locate
the primary source themselves (via the terms given in lecture) rather than rely on an invented
citation; every result referenced in this course is real, well-established, and correctly
attributed to its actual authors or research groups.

## 10. Academic Integrity

Labs and assignments are individual unless stated otherwise. The research proposal capstone is
individual work, reflecting its role as a qualifying-exam-style assessment of each student's own
ability to formulate research; any collaboration (e.g., discussing a problem statement with a
peer) must be disclosed in the proposal's acknowledgments. Postgraduate work is held to an
**original-contribution standard**: a related-work survey must accurately represent what each
cited paper actually claims and found, a proposed approach must be the student's own formulation,
and any feasibility argument or preliminary result must be the student's own reasoning or own
experimentation. Uncredited reuse of another author's ideas, text, code, or results — including
uncredited AI-generated text or code submitted as original work, or presenting a paper's reported
results as the student's own preliminary findings — is handled per institutional academic
integrity policy and, for the capstone specifically, is treated with the same seriousness as
plagiarism in a thesis proposal.
