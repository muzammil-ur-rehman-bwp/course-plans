# Assignment 1 — SDE Diffusion, Advanced Diffusion Techniques, and Mixture-of-Experts (Weeks 2–4)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 4 |
**Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(SDE discretization, 20 pts)** Starting from the VP-SDE $dx = -\tfrac12\beta(t)x\,dt +
   \sqrt{\beta(t)}\,dw$, derive its Euler-Maruyama discretization and show it reduces to DDPM's
   forward step $x_t \approx \sqrt{1-\beta_t}\,x_{t-1} + \sqrt{\beta_t}\,\epsilon$ for
   $\beta_t := \beta(t)\Delta t$. Implement a score network and train it via denoising score
   matching on a provided 2-D three-component Gaussian mixture, and implement an Euler-Maruyama
   reverse-SDE sampler, reporting a scatter plot of 1,000 generated samples overlaid on the true
   density contours.
2. **(Classifier-free guidance, 20 pts)** Derive the classifier-free guidance formula from the
   implicit-classifier substitution. Extend your Question 1 score network to a conditional model
   with condition dropout, and sample at $w \in \{1, 2, 5, 10\}$, reporting how sample spread
   (a simple variance-based diversity measure) changes with $w$ and relating the trend to your
   derivation.
3. **(Flow matching, 20 pts)** Implement the flow-matching training loop and ODE sampler from the
   Week 3 lecture content on the same mixture data. Report the smallest `n_steps` that gives
   samples visually comparable in quality to your Question 1 sampler's 1,000-step output, and
   explain in 3–5 sentences why flow matching can require fewer integration steps.
4. **(Mixture-of-Experts, 25 pts)** Implement a top-$k$ MoE layer ($N=8$, $k=2$) and a
   load-balancing auxiliary loss on a provided toy sequence-classification task. Report the
   per-token FLOP/parameter accounting against a matched dense baseline, and the per-expert usage
   histogram with and without the load-balancing loss.
5. **(Synthesis, 15 pts)** In 200–300 words, state precisely how this assignment's three
   generative-modeling questions (SDE diffusion, classifier-free guidance, flow matching) relate
   to each other as a family of continuous-time generative frameworks, and how MoE's sparse-
   activation idea (Question 4) is a conceptually distinct kind of efficiency gain (architectural
   sparsity) from either.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
