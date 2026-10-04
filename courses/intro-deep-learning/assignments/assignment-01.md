# Assignment 1 — CNN Arithmetic and Architectures (Weeks 3–4)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 4 | **Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (code + brief explanation) for each question.

## Questions
1. **(Convolution arithmetic, 20 pts)** For five provided (input size, kernel size, stride,
   padding) configurations, compute the output size by hand, then verify each with `nn.Conv2d`.
   Also compute the receptive field after 4 stacked layers of kernel size 3, stride 1.
2. **(Parameter counting, 15 pts)** For a 64×64×3 input and a target of 32 output feature maps,
   compute and compare the parameter count of a fully-connected layer vs. a `Conv2d(3, 32,
   kernel_size=3)` layer producing the same number of output channels; explain the difference in
   terms of parameter sharing.
3. **(Architecture evolution, 20 pts)** In 150–250 words, explain the degradation problem in very
   deep plain networks and how ResNet's skip connections address it, referencing the identity-
   mapping argument from lecture.
4. **(Residual CNN, 30 pts)** Build a small CNN with at least one residual block; train it on
   CIFAR-10 or Fashion-MNIST for at least 10 epochs; report training/validation curves and final
   test accuracy.
5. **(Comparison, 15 pts)** Compare Question 4's residual CNN against a plain CNN of similar
   depth/parameter count (reusing Lab 4 if applicable); report both models' test accuracy and
   discuss which trained more stably.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
