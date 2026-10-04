# Assignment 1 — NTK, Mean-Field Theory, Implicit Bias, and Feature Learning (Weeks 2–5)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 5 |
**Due:** Start of Week 7

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(NTK and lazy training, 25 pts)** Implement `MLP`, `ntk_matrix`, and `run` exactly as in the
   Week 2 lecture content. Run the width sweep for at least 5 widths spanning at least two orders
   of magnitude, and plot relative parameter movement and kernel drift against width. In 3–5
   sentences, explain why both quantities should shrink as width grows, referencing the
   Hessian-shrinking argument from Week 2 §2.
2. **(Mean-field signal propagation, 20 pts)** Implement `propagate_variance` exactly as in the
   Week 3 lecture content. Reproduce the three traces (linear/Xavier, ReLU/He, ReLU/mis-scaled) and
   additionally find, by numerical search, the fixed-point $\sigma_w^2$ for leaky-ReLU with
   $\alpha=0.3$. In 2–3 sentences, state how your found value compares to the He ($\alpha\to0$) and
   linear ($\alpha\to1$) limits.
3. **(Max-margin implicit bias, 25 pts)** Implement the gradient-descent training loop and the
   penalized hard-margin solver exactly as in the Week 4 lecture content. Run gradient descent for
   at least 20,000 steps on a provided separable dataset, and plot cosine similarity to the
   max-margin direction over training. In 3–5 sentences, explain why the norm of $w_t$ grows only
   logarithmically in the number of steps, referencing Week 4 §3 Step 4.
4. **(Feature learning vs. kernel regression, 20 pts)** Implement `experiment` exactly as in the
   Week 5 lecture content, and run it for at least 5 widths spanning at least two orders of
   magnitude. Report kernel-regression test MSE, trained-network test MSE, and kernel drift at
   each width, and identify the width range where the trained-network advantage over kernel
   regression is largest.
5. **(Synthesis, 10 pts)** In 200–300 words, explain precisely how the NTK limit (Week 2) and the
   mean-field limit (Week 3) are both valid, but different, width-$\to\infty$ idealizations of the
   same wide network, and connect this distinction to your own Question 1 and Question 4 results.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
