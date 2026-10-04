# Assignment 1 — Universal Approximation and Automatic Differentiation (Weeks 2–3)

**Weight:** 5% of course grade (one of 3 problem-set assignments, 15% total) | **Assigned:** Week
3 | **Due:** Start of Week 5

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (derivations and/or code) for each question.

## Questions
1. **(Universal Approximation proof sketch, 20 pts)** Using the bump-function construction from
   Week 2, show explicitly (with formulas and a plot) how two sigmoidal units can be combined to
   approximate the indicator function of the interval $(0.3, 0.7)$ on $[0,1]$, for sigmoid
   steepness $k \in \{5, 20, 100\}$. Discuss, in 2–3 sentences, how the approximation's sharpness
   changes with $k$.
2. **(Depth-vs-width tradeoff, 15 pts)** Implement a target function with at least 3 distinct
   length scales (e.g., a sum of sines at very different frequencies, or a piecewise sawtooth).
   Fit it with a wide-shallow network and a narrow-deep network of matched parameter count; report
   final training MSE for both and discuss which better captures the fine-scale structure.
3. **(Forward- vs. reverse-mode AD, 20 pts)** For $f(x_1,x_2,x_3) = x_1 x_2 + \sin(x_2 x_3)$,
   compute $\nabla f$ at $(x_1,x_2,x_3)=(1,2,0.5)$ **by hand** via forward-mode AD (one sweep per
   input) and **by hand** via reverse-mode AD (one sweep total); show every intermediate tangent/
   adjoint value, and confirm both methods agree with each other and with a direct symbolic
   derivative.
4. **(Autodiff engine extension, 25 pts)** Extend the Week 3 `Value` class with `log` and `exp`
   methods (correct backward closures). Use your extended engine to implement and verify, via
   finite differences, the gradient of the binary cross-entropy loss
   $L = -[y\log\hat y + (1-y)\log(1-\hat y)]$ with respect to a raw logit $z$ where
   $\hat y = \sigma(z)$, for 3 different $(y, z)$ pairs, including at least one pair where
   $z$ is large in magnitude (testing numerical stability).
5. **(Reflection, 20 pts)** In 6–8 sentences, explain why backpropagation's efficiency over naive
   forward-mode AD for neural network training is a direct consequence of networks having many
   parameters but one scalar loss output, connecting this explicitly to the general
   forward-vs-reverse-mode cost argument from lecture.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
