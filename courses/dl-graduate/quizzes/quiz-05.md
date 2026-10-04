# Quiz 5 — Deep Reinforcement Learning (Week 11)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. Write the DQN target $y$ for a transition $(s,a,r,s')$ in terms of the target network
   $Q_{\theta^-}$. (2 pts)
2. Name the two mechanisms DQN uses to address training instability, and which specific failure
   factor (from Week 10) each one addresses. (2 pts)
3. Write the REINFORCE gradient estimator, and name the trick used to derive it from
   $\nabla_\theta J(\theta)=\sum_\tau \nabla_\theta P_\theta(\tau)R(\tau)$. (2 pts)
4. Why does subtracting an action-independent baseline from the return leave the REINFORCE
   estimator unbiased? (2 pts)
5. In actor-critic methods, what does the critic learn, and how is the advantage $A(s,a)$ defined
   in terms of it? (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
