# Assignment 1 — Online Learning & Bandit Theory (Weeks 2–4)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 4 |
**Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Multiplicative weights, 25 pts)** Implement `multiplicative_weights(loss_matrix, eta)`
   exactly as in the Week 2 lecture content. Run it on a provided N=8-expert, T=3000-round loss
   matrix with η = √(ln 8 / 3000), and plot cumulative learner loss, the best fixed expert's
   cumulative loss, and the running regret. In 3–5 sentences, derive the optimized regret bound
   O(√(T ln N)) from Regret_T ≤ ηT + (ln N)/η by choosing η, and confirm your empirical regret
   stays below the theoretical bound at every round.
2. **(UCB1 and the Hoeffding bound, 20 pts)** Implement UCB1 on a provided K=6-arm Bernoulli
   bandit. Run it for two horizons, T=1000 and T=10000, and report empirical cumulative regret at
   each. In 2–3 sentences, relate the slower-than-linear growth you observe to the
   O((K log T)/Δ) expected-regret bound derived in Week 3.
3. **(Thompson sampling, 15 pts)** Implement Thompson sampling with a Beta-Bernoulli posterior
   for the same bandit as Question 2, and compare its cumulative regret curve to UCB1's on the
   same axes. In 3–5 sentences, explain conceptually why posterior sampling achieves exploration
   without computing an explicit confidence bound.
4. **(Contextual bandits, 25 pts)** Implement the simplified LinUCB-style algorithm from the
   Week 4 lecture content on a provided synthetic contextual bandit (d=5 context features, K=4
   arms, each with a known ground-truth linear reward weight vector). Compare its cumulative
   regret against a context-free UCB1 baseline that ignores the context, and report the ratio of
   final regrets.
5. **(Spectrum synthesis, 15 pts)** In 200–300 words, state precisely where tabular Q-learning
   sits on the bandit → contextual-bandit → MDP spectrum, in terms of state persistence and
   delayed reward, referencing concrete differences between your Question 2/3 implementations
   (no persisting state) and your Question 4 implementation (context persists only within a
   round, not across rounds).

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
