# Week 10 Summary — Interpretability Research

**Key takeaways:**
- Feature attribution methods (saliency, permutation importance, LIME/SHAP-style surrogates)
  attribute a prediction to input features but only claim local correlation/sensitivity.
- Mechanistic interpretability aims to reverse-engineer the actual circuits/algorithm a model has
  learned — a fundamentally stronger claim than attribution, and still an emerging research area.
- Sanity-check findings (e.g., insensitivity to parameter randomization) show some attribution
  methods can look plausible without reflecting the model's real computation — the post-hoc vs.
  mechanistic-understanding gap.

**You should now be able to:** implement permutation importance; run and interpret a
randomization sanity check; distinguish post-hoc attribution claims from mechanistic claims.

**Next week:** Foundational and philosophical debates I — the symbol grounding problem and the
frame problem, as live research questions.
