# Week 5 Summary — In-Context Learning Mechanics

**Key takeaways:**
- ICL is the phenomenon of a frozen, pretrained Transformer performing a new task from in-context
  examples alone, with no weight update — surprising relative to the standard fine-tuning picture.
- Induction heads (a previous-token head feeding an induction head) are a well-supported
  mechanistic finding, verified by direct weight analysis and ablation evidence linking them to
  ICL capability specifically.
- The implicit-gradient-descent analogy is a more speculative theory, rigorously shown only for
  restricted (linear-attention, linear-regression) toy settings; extrapolating it to general ICL
  in real Transformers outruns current evidence.
- A structured evidence-grading standard (what was shown, under what assumptions, what gap
  remains) is this week's transferable skill, reused later in the course.

**You should now be able to:** state the ICL phenomenon precisely; trace an induction-head circuit
on a toy sequence; critically separate well-supported mechanistic claims from speculative
extrapolations.

**Next week:** Preference-based alignment I — the Bradley-Terry model for pairwise human
preferences and the RLHF pipeline's three-stage structure.
