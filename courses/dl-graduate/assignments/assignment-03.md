# Assignment 3 — Deep Reinforcement Learning (Weeks 10–11)

**Weight:** 5% of course grade (one of 3 problem-set assignments, 15% total) | **Assigned:** Week
11 | **Due:** Start of Week 13

## Instructions
Submit a single Jupyter notebook `assignment03.ipynb` answering all questions below. Show your
work (derivations and/or code) for each question.

## Questions
1. **(DQN target derivation, 15 pts)** Given a transition $(s,a,r,s')$ with $\gamma=0.95$,
   $r=2$, and $\max_{a'}Q_{\theta^-}(s',a')=10$, compute the DQN target $y$ by hand; then, given
   $Q_\theta(s,a)=8$, compute the squared-error loss term.
2. **(DQN implementation and ablation, 30 pts)** Implement DQN with experience replay and a
   target network on a toy environment; run the full method and at least one ablation (no
   target network, or no replay buffer); report and compare training curves; explain the observed
   (or absent) instability in terms of the three failure factors from lecture.
3. **(REINFORCE derivation, 20 pts)** Starting from $J(\theta)=\sum_\tau P_\theta(\tau)R(\tau)$,
   derive the REINFORCE gradient estimator step by step, explicitly showing where the
   log-derivative trick is applied and where the environment's transition probabilities drop out.
4. **(REINFORCE implementation, 20 pts)** Implement REINFORCE with a return baseline on a toy
   environment; report the learning curve and the measured variance reduction (with vs. without
   the baseline) for a fixed policy snapshot, as in Lab 11 Task C.
5. **(DQN vs. REINFORCE, 15 pts)** In 150–250 words, compare DQN and REINFORCE: what each
   directly learns (a value function vs. a policy), and one concrete scenario where one would be
   preferred over the other.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
