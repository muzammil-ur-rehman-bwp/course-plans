# Research Capstone — Proposal Guidelines

**Topic selection opens:** Week 7–8 (informal discussion with instructor) | **Formal proposal
due:** End of Week 8–9 | **Weight:** part of the Research Capstone (25% of course grade total)

## What to Submit
A 1–2 page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Subtopic:** which neural-network-theory subtopic from this course (or closely adjacent, with
   instructor approval) the project addresses — e.g., approximation theory, automatic
   differentiation, initialization/normalization theory, optimization-landscape theory,
   generalization theory, the Neural Tangent Kernel, the Lottery Ticket Hypothesis, or
   information-bottleneck perspectives.
2. **Motivating question:** what specific question or gap is the project investigating? (This need
   not be fully refined yet — Week 15's work session will sharpen it — but it must be more
   specific than "I want to study generalization.")
3. **Planned literature:** 3–5 candidate papers (title/authors/venue/year, as best known) the
   student plans to review; these may change as the literature review develops, but the proposal
   must show a genuine starting point, not a placeholder.
4. **Planned experiment or derivation:** a reproduced or extended experiment from one of the
   candidate papers, or a focused derivation/comparison using techniques from this course — stated
   concretely enough that a feasible scope is clear (e.g., "reproduce the double-descent curve for
   a 2-layer MLP under 3 noise levels and compare the interpolation-threshold location to the
   theoretical prediction from [paper]" is concrete; "study generalization" is not).
5. **Team:** individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
build on techniques covered in this course; topics requiring deep architectural content (CNNs,
RNNs, Transformers in depth) or classical statistical ML algorithms — owned by the sibling
graduate Deep Learning and Machine Learning courses — require instructor pre-approval and must
still center on this course's own theoretical techniques.

## Example Topics (for inspiration, not a closed list)
Reproducing a small-scale NTK computation across widths and empirically characterizing how
quickly trained-network predictions converge to kernel-regression predictions; an empirical study
of the Lottery Ticket Hypothesis's initialization-dependence claim across two different pruning
schedules; a controlled double-descent study comparing the interpolation-threshold peak's severity
under different levels of label noise; an empirical comparison of Rademacher-complexity-style
capacity estimates against actual generalization gaps across several network widths; a focused
critique-and-partial-reproduction of a specific published information-bottleneck experiment,
including the estimator-sensitivity analysis from Week 14; a study of gradient-flow magnitude
through depth for several skip-connection variants, extending the Week 11 lab.
