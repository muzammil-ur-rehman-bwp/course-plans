# Week 6 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Preference-Based Alignment I — The Bradley-Terry Model and the RLHF Pipeline

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Derive the Bradley-Terry pairwise-preference probability formula from the odds-ratio
   assumption. (*Analyze*)
2. Derive the reward-model logistic loss from the Bradley-Terry model. (*Apply*)
3. State the three-stage RLHF pipeline and write the KL-regularized stage-3 objective. (*Apply,
   Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why pairwise human comparisons are easier to collect reliably than absolute scores |
| 0:15–0:45 | Bradley-Terry | Odds-ratio derivation of $P(y_1\succ y_2\mid x)=\sigma(r(x,y_1)-r(x,y_2))$ |
| 0:45–1:10 | Reward-model loss | The logistic/cross-entropy loss over preference pairs, derived from maximum likelihood |
| 1:10–1:45 | The RLHF pipeline | SFT → reward model → RL fine-tuning; the KL-regularized stage-3 objective, written out term by term |
| 1:45–2:00 | Synthesis | Why $\beta=0$ breaks the pipeline; preview: PPO optimizes this objective (Week 7) |

### Materials/Equipment
- Slides: "Preference-Based Alignment I"
- Whiteboard for the Bradley-Terry and reward-loss derivations
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Given a toy reward function $r(x,y)=\theta^\top \phi(x,y)$ and three preference pairs, compute the
Bradley-Terry log-likelihood by hand and state the gradient's sign intuition (which direction
$\theta$ moves for a given pair).

### Link to Lab/Assessment
Lab 6: implement a reward model and train it via the Bradley-Terry logistic loss on synthetic
preference pairs with a known ground-truth reward (see `lab-manuals/lab-06.md`).
