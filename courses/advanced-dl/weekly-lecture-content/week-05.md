# Week 5 — Lecture Content: In-Context Learning Mechanics

## 1. The Phenomenon
Give a frozen, pretrained decoder-only Transformer a prompt consisting of a handful of
input-output example pairs for some task (e.g., "France → Paris, Japan → Tokyo, Germany → ?") and
it often produces the correct completion — **with no gradient update to any parameter**. This is
**in-context learning (ICL)**. It is scientifically surprising against the graduate course's
standard picture of adaptation (pretrain, then fine-tune by updating weights on new data): here,
every weight is frozen, yet the model's behavior changes with the prompt as if it had "learned" the
task from the examples. The operative claim to hold onto precisely: nothing in the network's
parameters changes; whatever "learning" is happening is implemented entirely by the forward pass
over the context.

## 2. Induction Heads — A Well-Supported Mechanistic Finding
Direct mechanistic-interpretability analysis of small Transformers (inspecting attention patterns
and the weights that produce them, not just input-output behavior) has identified a concrete
circuit, built from **two attention heads in sequence**, that implements an approximate
"complete the pattern" operation:
- A **previous-token head** (in an earlier layer) writes, into each position's residual stream, a
  representation of *which token preceded it*.
- An **induction head** (in a later layer), reading that representation, attends back to the
  token that followed the *most recent prior occurrence* of the current token, and copies that
  token forward as its prediction.

Concretely, on a sequence like `... A B ... A`, having seen `A` followed by `B` earlier, an
induction head at the final `A` attends to the position right after the earlier `A` (i.e., to
`B`) and boosts the prediction of `B` as the next token. This is a **specific, falsifiable
algorithmic claim** — not merely "attention looks interesting here" — and it has been verified
with converging evidence: (a) directly inspecting the relevant heads' query-key weight structure
shows they implement exactly this copy-via-previous-occurrence mechanism; (b) **ablating**
(zeroing out) the identified induction heads disproportionately harms few-shot ICL accuracy on
held-out tasks relative to its effect on general next-token log-likelihood, directly linking the
circuit's presence to the ICL capability specifically, not merely to "the model got worse
overall."

```python
import torch

def toy_induction_trace(seq, vocab):
    """Given a token sequence (list of ints) and its vocab size, return, for each position i, the
    position an idealized induction head would attend to (the token right after the most recent
    PRIOR occurrence of seq[i]), or None if no prior occurrence exists."""
    last_seen = {}       # token -> most recent position it occurred at (before current scan point)
    targets = [None] * len(seq)
    for i, tok in enumerate(seq):
        if tok in last_seen:
            prev_pos = last_seen[tok]
            if prev_pos + 1 < len(seq):
                targets[i] = prev_pos + 1          # position of the token that followed last time
        last_seen[tok] = i
    return targets

seq = [3, 7, 1, 9, 3, 7, 1]       # "A B C D A B C" with A=3,B=7,C=1,D=9
print(toy_induction_trace(seq, vocab=10))
# position 4 (second '3'/'A') -> points at position 1 (where '7'/'B' is): predicts 'B' next — correct.
```

## 3. The Implicit-Gradient-Descent Analogy — Presented as More Speculative
A separate line of work asks a more ambitious question: can ICL's forward pass be shown
*mathematically equivalent* to an optimization procedure — specifically, one or more steps of
gradient descent on an implicit per-task loss, computed using the in-context examples as implicit
training data? The cleanest existing demonstrations of this equivalence are for **restricted toy
settings**: a single linear self-attention layer (attention without the softmax nonlinearity, or
with a specific simplification) processing in-context examples for a linear-regression task can be
shown, under these assumptions, to compute an update to its implicit "weights" that matches one
step of gradient descent on the squared-error loss over the in-context examples.

**What this toy result establishes:** for linear attention and a linear-regression-shaped
in-context task, a specific, exactly-characterizable mathematical equivalence to gradient descent
holds. **What it does not, by itself, establish:** that a real, deep, nonlinear, softmax-attention
Transformer performing ICL on realistic (non-linear-regression) tasks is "literally doing gradient
descent" in any precise sense. The assumptions doing the work — linearity of attention,
linearity of the task, a single layer — are each a substantial departure from a real pretrained
language model. Treating the toy equivalence as a general explanation of ICL, without qualifying
that it has only been shown in this restricted regime, outruns the current evidence. This is the
operative contrast with Section 2: induction heads are a mechanistic claim with direct structural
and ablation evidence in real, trained (if small) Transformers; the implicit-gradient-descent
picture is a suggestive, partially-proven analogy whose full generality to real large-scale ICL
remains an open research question (returned to explicitly in Week 14).

## 4. A Structured Standard for Evidence-Grading
This week's graded skill is not "memorize the induction-head circuit" but **learning to
distinguish, explicitly, what a result establishes from what it is popularly taken to establish**
— a skill this course will reuse repeatedly (Weeks 11, 12, 14 all contain claims of varying
evidentiary strength). A useful checklist: (1) What exactly was measured or proven? (2) Under what
assumptions/scale/setting? (3) What is the gap between that and the general claim being informally
made? (4) What evidence, if it existed, would close that gap?

## 5. In-Class/Lab Exercise
Run `toy_induction_trace` on several synthetic repeated-pattern sequences and confirm it correctly
identifies the induction target at each repeated occurrence. Then, using the Section 3 checklist,
write a 150–200 word structured critique of the implicit-gradient-descent analogy, explicitly
separating (a) what the linear-attention/linear-regression equivalence proves, (b) what additional
assumption would be needed to extend it to a real decoder-only Transformer, and (c) what
experiment would provide genuine evidence for or against that extension.
