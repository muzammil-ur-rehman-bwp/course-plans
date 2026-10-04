# Assignment 3 — MDPs, POMDPs, Probabilistic Inference (Weeks 8–11)

**Weight:** 5% of course grade (one of 3 problem sets, 15% total) | **Assigned:** Week 11 |
**Due:** Start of Week 13

## Instructions
Submit a single Jupyter notebook `assignment03.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Value and policy iteration, 25 pts)** For a provided 5x5 stochastic grid-world MDP,
   implement both value iteration and policy iteration. Confirm both converge to the same
   optimal policy. Report the number of iterations each takes to converge at γ = 0.9, and
   explain in 2–3 sentences why policy iteration's convergence guarantee (finite iterations)
   differs from value iteration's (asymptotic, contraction-based).
2. **(Q-learning, 20 pts)** Implement tabular Q-learning with ε-greedy exploration on the same
   grid-world (treating the model as unknown — your agent may only sample transitions, not query
   P or R directly). Report the learned policy's agreement with the value-iteration policy from
   Question 1, and the effect of ε = 0 vs. ε = 0.2 on learning (briefly).
3. **(POMDP belief updates, 20 pts)** For a provided 3-state POMDP (states, transition model,
   observation model given), implement the belief-state update and trace it over a provided
   sequence of 4 actions/observations. Report the final belief and explain, in 2–3 sentences,
   why the belief never reaches exact certainty given the observation model provided.
4. **(Approximate inference, 20 pts)** For a provided 5-node Bayesian network, implement
   rejection sampling and likelihood weighting for a query with moderately rare evidence. Run
   each method 10 times with 5,000 samples and report mean and standard deviation of each
   estimate, identifying which method has lower variance.
5. **(Synthesis, 15 pts)** In 150–250 words, compare and contrast the "known model" assumption
   across this assignment's methods (value/policy iteration assume a known MDP model; Q-learning
   does not; POMDP belief updates assume a known transition/observation model but not the true
   state; Bayesian network inference assumes a known network structure/CPTs but not the query
   variable's value) — identify which assumption is weakest/strongest to defend as "realistic"
   for a real-world application of your choice.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
