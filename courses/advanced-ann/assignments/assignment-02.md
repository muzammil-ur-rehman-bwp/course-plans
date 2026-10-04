# Assignment 2 — Sharpness, Grokking, Scaling Laws, Statistical Physics, and Double Descent (Weeks 6–10)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 10 |
**Due:** Start of Week 12

## Instructions
Submit a single document `assignment02.ipynb` (code questions) plus, where a question is purely
written, the corresponding markdown cells within the same notebook. Show your work (code + a
brief written explanation) for each question.

## Questions
1. **(Sharpness and SAM, 20 pts)** Implement `top_hessian_eigenvalue` and `sam_step` exactly as in
   the Week 6 lecture content. Train one model with plain SGD and one with SAM on the same
   provided data for the same number of steps, and report training loss, test loss, and estimated
   sharpness for each. In 3–4 sentences, explain the reparameterization critique from Week 6 §3
   and why it complicates a purely causal reading of your result.
2. **(Grokking, 15 pts — written only)** In 300–400 words, precisely restate the grokking
   phenomenon's three-phase curve and both competing hypotheses from Week 7, and identify one
   specific piece of evidence that would favor one hypothesis over the other if observed, without
   asserting either as the settled explanation.
3. **(Scaling laws, 20 pts)** Implement `fit_power_law` exactly as in the Week 8 lecture content.
   Fit a power-law exponent to a provided loss-vs-model-size dataset, and repeat the fit after
   removing the two largest-size data points. In 2–3 sentences, discuss how the fitted exponent's
   reliability depends on the range of sizes available.
4. **(Statistical physics, 15 pts — written only)** In 250–350 words, state the spin-glass
   critical-point-index/energy correlation from Week 9 precisely, and identify two of the three
   specific limits from Week 9 §4 that most constrain how confidently this analogy should be
   applied to a real, finite, trained network.
5. **(Double descent, 20 pts)** Implement `make_data` and `train_random_feature_model` exactly as
   in the Week 10 lecture content. Run the width sweep and identify the interpolation threshold.
   In 3–4 sentences, explain why test error should peak near this threshold specifically, using
   the "poorly conditioned interpolating solution" argument from Week 10 §2.
6. **(Synthesis, 10 pts)** In 200–300 words, connect any two of Weeks 6–10's topics to each other
   in a way not already explicitly stated in the lecture content (e.g., how sharpness might relate
   to the double-descent interpolation threshold, or how scaling laws might relate to grokking's
   onset time), stating clearly which claims are your own reasoning versus directly from lecture.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
