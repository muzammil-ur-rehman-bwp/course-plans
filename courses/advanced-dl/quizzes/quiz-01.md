# Quiz 1 — Score-Based Diffusion and Advanced Diffusion Techniques (Week 3)

**Duration:** 15 minutes | **Format:** 5 short-answer/code questions | **Weight:** part of
Quizzes (10%, best 5 of 6 counted)

1. Write the VP-SDE's forward drift and diffusion coefficients, and state what they reduce to
   under Euler-Maruyama discretization. (2 pts)
2. State the reverse-time SDE and explain precisely what quantity sampling reduces to knowing.
   (2 pts)
3. Write the classifier-free guidance formula $\tilde s_\theta(x,t,c)$ in terms of
   $s_\theta(x,t,c)$ and $s_\theta(x,t,\varnothing)$, and state what happens at $w=0$ and $w=1$.
   (2 pts)
4. True or false, with one sentence of justification: flow matching requires computing a
   trace-of-Jacobian term during training, the same as a normalizing flow trained by maximum
   likelihood. (2 pts)
5. A colleague claims: "Classifier-free guidance always improves sample quality, so $w$ should be
   set as large as possible." Identify precisely what is wrong with this claim. (2 pts)

**Answer key available to instructors in the course LMS gradebook (not distributed to students
before the quiz).**
