# Assignment 2 — Preference-Based Alignment and Efficient Inference (Weeks 6–9)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 9 |
**Due:** Start of Week 11

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Bradley-Terry and RLHF, 20 pts)** Derive the Bradley-Terry pairwise-preference formula from
   the odds-ratio assumption. Train a reward model via the Bradley-Terry logistic loss on a
   provided synthetic preference dataset (known ground-truth reward, Bradley-Terry-consistent
   noise), and report the fitted model's Spearman rank correlation against the ground truth on a
   held-out set. Write out the full stage-3 KL-regularized RLHF objective and state, in 2–3
   sentences, what concretely goes wrong if $\beta=0$.
2. **(DPO derivation, 25 pts)** Derive the DPO loss end to end: the closed-form optimal policy for
   the KL-regularized objective, the implied-reward inversion, and the substitution into
   Bradley-Terry showing the $Z(x)$ cancellation. Implement the DPO loss and train a small policy
   on the Question 1 preference data; report its implied-reward ranking's Spearman correlation
   against the ground truth and compare it to Question 1's explicit reward model's correlation.
3. **(Quantization, 20 pts)** Implement per-channel post-training quantization at $b\in\{8,4,2\}$
   bits on a provided trained network's weights, reporting the accuracy drop at each bit-width.
   Implement quantization-aware training at $b=4$ using a straight-through estimator and report
   the accuracy recovered relative to plain PTQ at the same bit-width.
4. **(Speculative decoding, 20 pts)** Implement the accept/reject/resample loop from the Week 9
   lecture content on a provided toy draft/target categorical-model pair. Run 10,000 trials and
   empirically confirm the output distribution matches the target model's distribution. Measure
   the average number of accepted tokens per block for a close-agreement and a poor-agreement
   draft-model setting, and report the ratio.
5. **(Synthesis, 15 pts)** In 200–300 words, compare PPO-RLHF and DPO as two routes to the same
   underlying KL-regularized objective, and relate speculative decoding's exactness guarantee
   (Question 4) to quantization's explicit accuracy tradeoff (Question 3) — in particular, state
   precisely which of the two efficient-inference techniques changes the model's output
   distribution and which does not, and why that distinction matters in practice.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
