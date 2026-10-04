# Quiz 2 — Initialization Theory (Week 4)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of Quizzes (10%, best 5 of 6 counted)

1. Why does all-zero weight initialization prevent a layer's units from ever differentiating
   from one another during training? (2 pts)
2. For a layer with fan-in $n_{in}=64$, state the Xavier/Glorot-recommended $\mathrm{Var}(W)$
   assuming $n_{out}=64$ as well. (2 pts)
3. For the same fan-in, state the He-recommended $\mathrm{Var}(W)$, and the factor by which it
   differs from your answer to Question 2. (2 pts)
4. In one sentence, why does ReLU's zeroing of roughly half its inputs require a different
   initialization variance than tanh? (2 pts)
5. If activation variance is observed to shrink geometrically with depth under some
   initialization scheme, is the weight variance likely too large or too small? (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
