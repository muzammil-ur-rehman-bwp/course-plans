# Week 14 — Lecture Content: Current Open Problems Survey

**This week is explicitly flagged as covering a fast-moving area** — the specific set of open
questions below is refreshed each offering rather than fixed permanently by this syllabus. The
illustrative examples below model the *kind* of question this week surveys.

## 1. Open Problem: Principled Load Balancing for Mixture-of-Experts
Week 4 introduced the auxiliary coefficient-of-variation loss as the standard practical mitigation
for router collapse. What is known: this auxiliary loss, tuned per-model, empirically flattens
expert-usage histograms in many settings. What is not known: whether a principled mechanism (one
that provably bounds load imbalance, or that does not require per-model retuning of an auxiliary
loss coefficient) can replace it without sacrificing accuracy — current practice still largely
relies on empirically-tuned auxiliary losses and heuristics (noisy gating, capacity limits) rather
than a mechanism with a clean theoretical guarantee. Progress would look like: a routing scheme
with a provable load-balance bound, demonstrated not to cost accuracy relative to the standard
auxiliary-loss approach across multiple model scales.

## 2. Open Problem: How Far Does the Induction-Heads/Implicit-GD Picture Generalize?
Week 5 distinguished induction heads (well-supported, mechanistic, ablation-verified for
pattern-completion-style ICL) from the implicit-gradient-descent analogy (rigorously shown only
for restricted linear-attention/linear-regression toy settings). What is known: induction heads
demonstrably contribute to *some* ICL behaviors in *some* trained Transformers; the implicit-GD
equivalence holds exactly in the stated toy settings. What is not known: whether a single
mechanistic account (induction heads, implicit optimization, or some other circuit entirely) can
explain the *full range* of tasks large pretrained models exhibit ICL on — current evidence is a
patchwork of specific findings in specific settings, not a unified theory, and it remains unclear
whether one is even the right shape of answer to look for. Progress would look like: a mechanistic
account, tested against ablation evidence, that correctly predicts which tasks a given model will
or will not show strong ICL on.

## 3. Open Problem: DPO's Robustness to Bradley-Terry Violations
Week 7 derived DPO as an exact reparameterization **under the Bradley-Terry assumption** that
preferences are odds-ratio-consistent with some latent scalar reward. What is known: real human
preference data sometimes violates this assumption (e.g., genuinely inconsistent or
multi-dimensional preferences that no single scalar reward can represent exactly, or
annotator-dependent preference criteria). What is not known: precisely how DPO's fitted policy
degrades when the assumption is violated by a given amount, and what a principled fix looks like
(a more general preference model with its own tractable closed-form reparameterization, if one
exists). Progress would look like: a characterization of DPO's failure mode under a specific,
well-defined class of Bradley-Terry violations, ideally with a proposed alternative loss that
provably degrades more gracefully.

## 4. A Fourth Candidate (Instructor-Refreshed)
A current question connecting Week 9's speculative decoding to Week 10's NAS or Week 4's MoE
(e.g., whether jointly training draft and target models, rather than treating the draft model as
fixed and independently trained, can push acceptance rates further without compromising the
exactness guarantee of Week 9 Section 3) — selected and refreshed by the instructor each offering
to keep this week's survey current.

## 5. How to Engage With an Open Problem (Reused Standard)
For each surveyed question, apply the same discipline used throughout the course (Weeks 5, 12):
state precisely what the strongest current partial answer establishes, and distinguish that from
what it is sometimes informally taken to establish. A genuinely open problem is not "nobody has
tried yet" — it is a question where a specific, identifiable technical obstruction remains even
after real attempts.

## 6. In-Class/Lab Exercise
Choose one surveyed open problem (or the instructor-refreshed fourth) and write a structured
250–350 word statement: (1) what is currently known, with its evidentiary basis; (2) what remains
unknown, stated precisely; (3) what a resolving (or significantly progressing) piece of evidence
would look like; (4) how this question relates, if at all, to your own emerging capstone interest.
