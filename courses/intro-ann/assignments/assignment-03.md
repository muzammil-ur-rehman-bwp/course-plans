# Assignment 3 — Optimizers & Regularization (Weeks 9–11)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 11 | **Due:** Start of Week 13

## Instructions
Submit a single Jupyter notebook `assignment03.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Initialization, 15 pts)** Implement Xavier and He initialization (if not reusing Lab 9
   code). For a 5-layer sigmoid network and a 5-layer ReLU network (10 units/layer), apply the
   initializer matched to each activation and report each layer's activation standard deviation
   for a fixed random input batch. Then deliberately apply He init to the sigmoid network and
   Xavier init to the ReLU network, and report the same statistics; comment on the difference.
2. **(Adam by hand, 20 pts)** For the scalar gradient sequence $g_1=1.0, g_2=-0.5, g_3=0.8,
   g_4=0.2$ and $\theta_0=0$, compute Adam's $m_t,v_t,\hat m_t,\hat v_t,\theta_t$ by hand for
   $t=1,\dots,4$ using default hyperparameters ($\beta_1=0.9,\beta_2=0.999,\eta=0.001,
   \delta=10^{-8}$); show every intermediate value.
3. **(Optimizer comparison, 25 pts)** Implement momentum, RMSProp, and Adam (if not reusing Lab 10
   code); train a from-scratch network on a provided dataset `assignment03_data.csv` with each of
   the four optimizers (including plain SGD), fairly tuning each optimizer's learning rate; plot
   all four loss curves and report which converged fastest/most stably.
4. **(Regularization comparison, 25 pts)** Using your best optimizer from Question 3, train three
   versions of the network on `assignment03_data.csv`: no regularization, L2 ($\lambda=0.05$),
   and dropout ($p=0.3$); plot training/validation loss for all three and report which generalizes
   best, citing the final train/validation gap for each.
5. **(Reflection, 15 pts)** In 4–6 sentences, explain how you would decide, for a new dataset,
   whether to reach for more regularization, a different optimizer, or more training data, given
   a specific training/validation curve pattern you describe.

## Submission
Upload `assignment03.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
