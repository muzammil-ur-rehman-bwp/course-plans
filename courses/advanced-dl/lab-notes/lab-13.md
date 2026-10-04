# Lab Notes 13 — Critiquing a Systems Paper and Starting the Capstone Survey

**Concept recap:** a systems-and-methods paper's claim must be read with its exact configuration
attached; benchmark culture and DL-specific reproducibility obstacles are named precisely, not
treated as vague worries; a problem statement must pass the precision test.

**Common pitfalls:**
- In Task A, stating a claim without its configuration ("the method is faster") rather than the
  precise, configuration-attached version ("2.1× faster at batch size 32 on a single A100,
  relative to the paper's own baseline implementation").
- In Task C, choosing candidate papers too broadly related to the topic area rather than
  genuinely informative for the eventual survey — prefer papers you can state a specific
  claim-and-finding for over papers chosen only by keyword match.
- In Task D, writing a problem statement that is really a topic in disguise ("I want to study
  speculative decoding") — re-apply the precision test explicitly: would a reader know, from this
  statement alone, what evidence would resolve it?
- Treating peer feedback on the problem statement as optional or skippable — the precision test
  is far more reliably applied by someone other than the author, who has no stake in defending an
  imprecise formulation.

**Debugging tip:** if you are unsure whether your problem statement is precise enough, try
writing down, in one sentence, what a "yes" answer and a "no" answer would each look like — if
you cannot write both, the statement is not yet precise enough.

**Instructor tip:** during the Task D peer-feedback exchange, circulate and listen for students
accepting a vague problem statement from a peer without pushing back — this is the most common way
the precision test fails to do its job in practice, and worth correcting live rather than only in
written feedback on Lab 13's submission.
