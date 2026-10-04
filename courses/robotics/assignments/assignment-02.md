# Assignment 2 — Control & Vision (Weeks 10–11)

**Weight:** 5% of course grade (one of 4 assignments, 20% total) | **Assigned:** Week 11 | **Due:** Start of Week 13

## Instructions
Submit `assignment02.ipynb`/package with working code and written answers for all questions.

## Questions
1. **(PID design, 20 pts)** Design a PID controller for a given set-point task (provided
   simulation scenario); tune it and report your final gains with a response plot.
2. **(PID analysis, 20 pts)** Starting from your tuned controller, deliberately set `Ki` to a
   value 10x too high; show and explain the resulting instability/oscillation in a plot.
3. **(Vision pipeline, 20 pts)** Build an HSV-threshold + contour-detection pipeline for a
   provided colored target; report the detected bounding box for 3 provided test frames.
4. **(Vision robustness, 20 pts)** Test your pipeline on 2 additional frames with different
   lighting; report whether detection succeeded, and if not, what threshold adjustment would fix
   it.
5. **(Integration reflection, 20 pts)** Write 4–6 sentences on how you would combine the vision
   detection (Question 3/4) with the PID controller (Question 1) to make a robot drive toward a
   detected object — you do not need to implement this, just describe the approach.

## Submission
Upload your notebook/package via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
