# Week 7 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: Preference-Based Alignment II — PPO for RLHF and Direct Preference Optimization

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State PPO's clipped surrogate objective and explain its role in RLHF fine-tuning, building
   explicitly on (not re-deriving) the graduate course's actor-critic foundation. (*Apply*)
2. Derive the KL-regularized objective's closed-form optimal policy. (*Analyze*)
3. Derive the DPO loss by substituting the implied reward into the Bradley-Terry loss, and show
   the partition function cancels. (*Analyze*)
4. Implement the DPO loss and train a small policy on synthetic preference pairs. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | PPO for RLHF | The clipped surrogate objective applied to the KL-regularized reward; why four interacting networks make this engineering-heavy |
| 0:20–0:40 | Motivating DPO | Why we'd like to skip the reward model and RL optimizer entirely |
| 0:40–1:10 | The closed-form optimal policy | Deriving $\pi^* \propto \pi_{\mathrm{ref}} \exp(r/\beta)$ on the board |
| 1:10–1:40 | The DPO loss | Implied-reward inversion, substitution into Bradley-Terry, $Z(x)$ cancellation — full derivation |
| 1:40–2:00 | Synthesis | What DPO gives up; recap table: RLHF-with-PPO vs. DPO |

### Materials/Equipment
- Slides: "Preference-Based Alignment II: PPO and DPO"
- Whiteboard for the full DPO derivation
- Live-coding environment (Jupyter, PyTorch)

### Formative Check (in-class)
Starting from $J(\pi) = \mathbb E_\pi[r(y)] - \beta\,\mathbb E_\pi[\log(\pi(y)/\pi_{\mathrm{ref}}(y))]$,
show $J(\pi) = -\beta\,D_{\mathrm{KL}}(\pi \,\|\, \pi_{\mathrm{ref}}\exp(r/\beta)/Z) + \beta\log Z$
and identify the maximizer.

### Link to Lab/Assessment
Lab 7: implement the DPO loss and train a small policy against a frozen reference policy on
synthetic preference pairs (see `lab-manuals/lab-07.md`).
Quiz 3 this week (Weeks 6–7 content).
