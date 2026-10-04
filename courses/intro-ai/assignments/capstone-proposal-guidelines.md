# Capstone Project — Proposal Guidelines

**Due:** Week 11 | **Weight:** 2% of the Capstone grade (20% total course weight)

## What to Submit
A 1-page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Problem statement**: what are you trying to solve (play a game well, diagnose a narrow
   domain, construct a plan, infer under uncertainty)?
2. **Classical technique**: exactly **one** classical AI technique from this course that your
   project is built on — search (uninformed/informed/adversarial), logic-based inference,
   classical planning (STRIPS), or probabilistic reasoning (Bayes' rule / Bayesian networks).
   This must **not** be a scikit-learn machine-learning project.
3. **Domain/scope**: the specific game, knowledge domain, planning problem, or diagnostic
   scenario you will build on, with enough detail that scope is clear and achievable in the time
   available (e.g., "Tic-Tac-Toe with a minimax agent," not "a general game-playing AI").
4. **Evaluation plan**: how will you demonstrate your system works (e.g., it never loses at
   Tic-Tac-Toe against optimal play; it correctly answers 10 test queries against a hand-checked
   knowledge base; it finds a correct plan for 3 test scenarios; its posterior probabilities
   match hand-computed values)?
5. **Team**: individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
use techniques covered in this course; techniques beyond the syllabus require instructor
pre-approval and are not required for full credit.

## Example Topics (for inspiration, not a closed list)
A minimax/alpha-beta game-playing agent (Tic-Tac-Toe, Connect Four, or Nim); a small logic-based
expert system for a narrow domain (e.g., simple animal identification, basic fault diagnosis via
forward/backward chaining); a toy STRIPS-style planner for an extended blocks-world or small
logistics domain; a Bayesian-network-based diagnostic tool for a small medical or
fault-diagnosis scenario (4–6 nodes).

**Explicitly out of scope for this capstone:** training a scikit-learn classifier/regressor on
a dataset, or any project whose core technique is from Weeks 12–15's survey (machine learning,
neural networks, NLP, vision, or robotics) rather than Weeks 3–11's classical techniques. That
kind of project belongs to the applied *Programming for Artificial Intelligence* course instead.
