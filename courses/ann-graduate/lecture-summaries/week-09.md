# Week 9 Summary — Midterm Exam; Generalization Theory I

**Key takeaways:**
- PAC learning formalizes learnability as finding, with high probability, a low-true-error
  hypothesis from polynomially many samples.
- VC dimension measures a hypothesis class's capacity via shattering; linear threshold classifiers
  in $\mathbb{R}^2$ have VC dimension exactly 3.
- The classical VC generalization bound becomes vacuous when VC dimension (which scales with
  parameter count for networks) far exceeds the training set size — a regime deep learning
  routinely operates in, yet still generalizes well, which is the open puzzle this unit sets up.

**You should now be able to:** state the PAC framework, compute the VC dimension of a simple
hypothesis class via shattering, and explain why classical VC bounds fail to explain deep learning.

**Next week:** generalization theory II — Rademacher complexity, margin bounds, and the
random-label-fitting phenomenon.
