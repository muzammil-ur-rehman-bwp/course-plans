# Quiz 3 — Bradley-Terry, RLHF, PPO, and DPO (Week 7)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of
Quizzes (10%, best 5 of 6 counted)

1. Derive the Bradley-Terry formula $P(y_1\succ y_2\mid x) = \sigma(r(x,y_1)-r(x,y_2))$ from the
   odds-ratio assumption, showing the key algebraic step. (2 pts)
2. State the three-stage RLHF pipeline in order, and name what each stage produces. (2 pts)
3. Write the closed-form optimal policy $\pi^*(y\mid x)$ for the KL-regularized objective, and
   state what it reduces to when $r(x,y)=0$ for all $y$. (2 pts)
4. In the DPO derivation, explain precisely why the partition function $Z(x)$ cancels when
   substituting the implied reward into the Bradley-Terry loss. (2 pts)
5. A colleague claims: "DPO is only an approximation to RLHF, since it skips training a reward
   model." Identify precisely what is wrong with this claim. (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
