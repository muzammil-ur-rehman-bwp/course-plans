# Week 16 — Lecture Content: Capstone Research Presentations; Course Review

## 1. Presentation Logistics
Each individual or pair presents their Research Capstone as a conference-style talk (time limit
and criteria per `assignments/capstone-rubric.md`), followed by audience Q&A. Presentations are
scheduled across both the lecture and seminar periods this week; see the posted schedule.

## 2. The Course Map, Revisited
Six pillars, each building on the last:
1. **Approximation theory** (Week 2) established that networks *can* represent essentially any
   continuous function — an existence result, not an account of learnability.
2. **Automatic differentiation** (Week 3) established *how* the gradients training needs are
   actually computed in general, with backpropagation as its specialization to layered networks.
3. **Initialization and normalization theory** (Weeks 4–5) established why the *starting point*
   and the *running statistics* of training must be chosen deliberately for signal to survive
   propagation through depth at all, and throughout training.
4. **Optimization-landscape theory** (Weeks 6–8) established what the loss surface actually looks
   like (saddle-dominated, not riddled with bad local minima) and why the practical optimizers
   from the prerequisite course behave, and sometimes fail, as they do.
5. **Generalization theory** (Weeks 9–11) established that classical capacity-based theory (VC
   dimension) cannot by itself explain deep networks' good generalization, motivating
   data-dependent (Rademacher, margin) and implicit-bias-flavored accounts instead.
6. **Modern research directions** (Weeks 12–14) examined three specific, technically grounded,
   still-active theories — NTK, Lottery Ticket, information bottleneck — each explaining *some*
   piece of the puzzle while explicitly not being a complete theory of deep learning.

The throughline: no single pillar, by itself, explains why deep learning works as well as it does
in practice — the honest, current state of the field is a collection of partial, rigorous, and
sometimes competing theoretical accounts, each covering part of the picture.

## 3. Where This Leads Next
- **Deep Learning, Graduate** takes the architectures this course deliberately set aside
  (CNNs, RNNs, Transformers, generative models) and studies them in full depth, now with this
  course's theoretical vocabulary (expressivity, optimization landscapes, generalization) as
  background for understanding *why* those architectural choices work, not just *how* to use them.
- **Machine Learning, Graduate** covers the classical statistical learning algorithms this course
  intentionally did not (SVMs, ensembles, etc.), with the generalization-theory vocabulary from
  Weeks 9–11 directly applicable there too.
- Continued reading in current theoretical ML venues (NeurIPS, ICML, and others) is the natural
  next step for anyone whose capstone sparked a direction they want to keep pursuing.

## 4. Course Review Exercise
In writing (submitted as this week's formative check), state, in one sentence each, what Weeks
2, 5, 8, 10, and 12 individually taught you to be skeptical of (a claim, an informal intuition, or
a classical result) that you would have taken at face value before this course.
