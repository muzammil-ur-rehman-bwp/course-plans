# Quiz 3 — Backpropagation (Week 8)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. State the general recursive backpropagation rule for $\delta^{(l)}$ in terms of
   $\delta^{(l+1)}$. (2 pts)
2. For cross-entropy loss paired with a sigmoid or softmax output, what is $\delta^{(L)}$ at the
   output layer? (2 pts)
3. Given $\delta^{(l)}$ and $a^{(l-1)}$, write the formulas for $\partial L/\partial W^{(l)}$ and
   $\partial L/\partial b^{(l)}$. (2 pts)
4. What does a gradient check (finite-difference comparison) verify, and why is it trustworthy
   evidence that a backward-pass bug is present if it fails? (2 pts)
5. Name one common bug in a from-scratch backpropagation implementation and describe the
   symptom it produces. (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
