# Week 14 — Lecture Content: Current Open Problems Survey

*This week is explicitly flagged as covering a fast-moving research area. The specific state of
"open-ness" for each question below may date quickly; the instructor refreshes the exact set of
questions and the papers representing their frontier each time this course is offered, rather than
this syllabus fixing them permanently. What follows is a grounded snapshot, not a permanent list.*

## 1. Open Question 1 — A Unified Theory of Feature Learning's Benefits
Week 5 established *that* finite-width networks escape the NTK/lazy regime into feature learning,
and gave empirical signatures of this. What remains open: a theory that goes beyond qualitative or
purely empirical characterization to **practically predictive** guidance — for example, predicting
in advance, for a given architecture/task/width combination, how much benefit feature learning
will provide over the kernel-regression baseline, rather than only measuring that benefit after
training. Current partial answers (extensions of mean-field theory to track leading-order
corrections beyond the strict infinite-width limit; specific tractable toy models where feature
learning's benefit can be computed exactly) establish the *mechanism* in restricted cases but do
not yet generalize to practically predictive statements for realistic architectures — the gap
between "we can explain this toy case" and "we can predict the realistic case in advance" is the
open problem, not the existence of feature learning's benefit itself (which is well established
empirically).

## 2. Open Question 2 — Bounds That Are Simultaneously Tight, Invariant, and Computable
Week 11 established PAC-Bayes bounds can be numerically non-vacuous, and Week 6 established that
standard sharpness measures are reparameterization-sensitive. These two facts are in tension: a
PAC-Bayes bound built on a Gaussian posterior centered at the trained weights (Week 11's
construction) inherits some sensitivity to the same parameterization choices that make raw
sharpness measures unreliable, since the posterior's covariance structure is defined in a specific
parameter space. What remains open: constructing a generalization bound that is simultaneously (a)
numerically tight (non-vacuous and close to the true generalization gap), (b) invariant to
function-preserving reparameterizations (not an artifact of parameterization choice), and (c)
practically computable without prohibitive cost — current work exists addressing any two of these
three properties, but a bound establishing all three simultaneously, for realistic deep networks,
is not currently available.

## 3. Open Question 3 — Grokking and Double Descent: One Mechanism or Two?
Both grokking (Week 7) and double descent (Week 10) produce non-monotonic curves in some training
quantity (test accuracy vs. training step for grokking; test error vs. capacity/data/time for
double descent) that superficially resemble each other — a quantity that looks poor for a stretch,
then improves. Some current work has asked whether epoch-wise double descent and grokking's long
plateau might be the same underlying mechanism (something about effective capacity or implicit
regularization changing slowly over training steps) viewed at different task/architecture/
hyperparameter settings, versus being genuinely distinct phenomena whose curves merely happen to
look superficially similar for unrelated reasons. This question is currently open: no proposed
unifying account has been broadly accepted as establishing the two phenomena share a single
mechanism, and no proposed separation argument has been broadly accepted as definitively ruling
out a shared mechanism either — stating this honestly as unresolved is itself the correct,
examinable answer at this stage, not a hedge to be improved upon.

## 4. The Discipline of Surveying Open Problems Honestly
For each question above, note the pattern this course has used consistently: state precisely what
is *established* (the phenomenon or mechanism in its demonstrated, restricted cases), then state
precisely what remains *open* (the gap between the restricted case and the general, practically
useful claim), without collapsing the two into either "nothing is known" or "this is basically
solved." This is the same discipline Week 13 teaches for scoping a research proposal's problem
statement, and these three questions are offered as concrete, course-connected starting points for
a capstone topic — not an exhaustive list of what is open in the field.

## 5. In-Class/Seminar Exercise
See `lab-manuals/lab-14.md`: choose one of the three open questions (or, with instructor approval,
a different current open question connected to this course's material) and write a position paper
stating precisely what is established, what remains open, and one concrete, falsifiable sub-
question from it that could plausibly seed a capstone problem statement.
