# Week 1 — Lecture Content: The Frontier Deep-Learning Landscape

## 1. Where This Course Sits
*Introduction to Deep Learning* (undergraduate) surveys CNNs, RNNs, attention, a Transformer
diagram, and basic generative models. *Deep Learning*, Graduate derives the Transformer from
scratch, goes deep on advanced CNNs, self-supervised/contrastive learning, discrete-time DDPM
diffusion, graph neural networks, deep RL (DQN, REINFORCE, actor-critic), and large-scale training
basics (parallelism, mixed precision, LoRA). **This course assumes every one of those as firm,
working background.** If you cannot currently derive multi-head attention from scratch, write the
DDPM training loss from memory, or explain why a target network stabilizes DQN, pause and revisit
the graduate course's materials before continuing — none of it will be re-taught here.

## 2. The Four Postgraduate Siblings
The postgraduate tier splits the frontier of AI/ML/DL research into four disjoint courses:

| Course | Owns |
|---|---|
| *Advanced Artificial Neural Network* | NTK in depth, mean-field theory, implicit bias, feature learning, sharpness/SAM, grokking, scaling laws (**theoretical**: why power laws hold), statistical-physics approaches, double descent, PAC-Bayes, mode connectivity |
| *Advanced Artificial Intelligence* | Regret/bandit theory, multi-agent RL, algorithmic game theory/mechanism design, AI safety/alignment as a **research-agenda/philosophical** topic, interpretability, foundational debates |
| *Advanced Machine Learning* | Minimax bounds, high-dimensional statistics, full-information online convex optimization, nonparametric Bayesian methods, causal inference, algorithmic fairness, robust statistics |
| **Advanced Deep Learning (this course)** | SDE-based score generative modeling, advanced diffusion, Mixture-of-Experts, in-context learning mechanics, preference-based alignment **engineering** (RLHF pipeline, PPO, DPO), efficient inference, NAS, scaling laws (**systems/engineering**: compute-optimal allocation in practice), a frontier generative-modeling survey |

Two boundaries are worth stating precisely, since they are the easiest to blur in casual
conversation about "scaling laws" or "alignment":
- **Scaling laws.** *Advanced Artificial Neural Network* asks *why* loss falls as a power law in
  model size/data/compute — data-manifold arguments, random-feature/kernel-theoretic arguments.
  This course's Week 11 takes the power law as an empirical given and asks a systems question:
  *given* a fixed compute budget, how should it be split between model size and data, and what
  real engineering (checkpointing, fault tolerance) does a large training run require? Different
  question, different answer, same underlying empirical phenomenon.
- **Alignment.** *Advanced Artificial Intelligence* treats alignment as a research agenda —
  specification gaming, outer/inner alignment, scalable oversight, interpretability-as-safety —
  largely independent of any one specific training algorithm. This course's Weeks 6–7 instead
  derive the concrete engineering pipeline that implements *one* popular approach to alignment in
  practice: Bradley-Terry reward modeling, the RLHF pipeline, PPO, and DPO. If you want to know
  *whether* reward modeling is philosophically adequate as a safety approach, that is the sibling
  course; if you want to know *how* to derive and implement it, that is here.

*Advanced Machine Learning*'s territory (minimax bounds, high-dimensional statistics, OCO,
nonparametric Bayes, causal inference, fairness, robust statistics) has no expected overlap with
this course at all — confirmed by inspection of both course-contents files.

## 3. This Course's Map
```
Weeks 1      Course overview / scoping
Weeks 2–3    Advanced generative modeling: SDE score-based diffusion, classifier-free
             guidance, flow matching
Week 4       Mixture-of-Experts and sparse architectures
Week 5       In-context learning mechanics
Weeks 6–7    Preference-based alignment engineering: Bradley-Terry, RLHF pipeline, PPO, DPO
Weeks 8–9    Efficient inference: quantization, distillation, speculative decoding (midterm Wk 9)
Week 10      Neural architecture search
Week 11      Scaling laws — systems/engineering perspective
Week 12      Frontier generative-modeling survey (fast-moving, flagged)
Week 13      Research methods for applied DL research
Week 14      Current open problems survey (fast-moving, flagged)
Weeks 15–16  Research proposal work session, capstone presentations
```
Each topic is chosen because it extends a specific piece of the graduate course rather than
standing apart from it: SDE diffusion extends DDPM; MoE extends the Transformer's feed-forward
sublayer; RLHF/DPO extends policy gradients/actor-critic; NAS and systems-scaling extend the
large-scale-training-basics week. Nothing here is free-standing trivia — each week's "why now"
should be traceable to a specific graduate-course result it builds on.

## 4. How to Use the Research-Proposal Structure (Previewed)
Every postgraduate course in this sequence ends in the same deliverable shape: a problem
statement, a related-work survey of 5+ papers, a proposed novel approach, and a feasibility
argument or preliminary result. We introduce it now, in one paragraph, because it should shape how
you read every week from here on — as you encounter each topic, ask yourself whether a specific,
falsifiable open question is lurking in it that you could eventually turn into your own capstone.
Week 13 gives the full treatment (precision tests, survey-synthesis standards, feasibility-
argument construction); Week 8 is an informal problem-statement check-in.

## 5. In-Class/Lab Exercise
Working individually, take the five paper titles distributed in lecture and, for each, write one
sentence stating which course (this one, or a named sibling) would own the paper's central
technical contribution, and name the specific weekly topic (this course) or pillar (sibling
course) it would fall under.
