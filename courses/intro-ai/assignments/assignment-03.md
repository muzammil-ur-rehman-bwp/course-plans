# Assignment 3 — Planning & Probabilistic Reasoning (Weeks 9–11)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 11 | **Due:** Start of Week 13

## Instructions
Submit `assignment03.ipynb` (or notebook + short written answers where indicated) with working
code and written answers for all questions.

## Questions
1. **(STRIPS Planning, 20 pts)** Define a small blocks-world domain (at least 4 STRIPS action
   schemas) with an initial state requiring a 3-step plan to reach the goal. Implement and run
   your forward state-space planner (from Lab 9) to find the plan; verify it is correct by
   applying each action in sequence.
2. **(Bayes' Rule, 20 pts)** A rare manufacturing defect occurs in 0.5% of produced items. An
   inspection process correctly flags defective items 98% of the time, and incorrectly flags
   non-defective items 3% of the time. Compute `P(defective | flagged)` by hand using Bayes'
   rule, then verify with code. Explain in 2–3 sentences why the result is lower than intuition
   might suggest.
3. **(Independence, 15 pts)** Given a small joint probability table (provided by the instructor)
   over two binary variables, determine whether they are independent, showing your work
   (`P(A,B)` vs. `P(A)P(B)`).
4. **(Bayesian Networks, 25 pts)** Given a 4-node Bayesian network (structure and CPTs provided),
   compute two different query probabilities by hand via enumeration, then verify both using
   your Lab 11 implementation.
5. **(Synthesis, 20 pts)** In 1–2 paragraphs, compare how STRIPS planning (deterministic,
   certain) and Bayesian networks (uncertain, probabilistic) each represent "what follows from
   what" — one via preconditions/effects, the other via conditional probability tables. What
   would have to change about STRIPS to handle actions with uncertain effects?

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
