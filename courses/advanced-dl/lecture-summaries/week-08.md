# Week 8 Summary — Quantization and Knowledge Distillation; Midterm Review

**Key takeaways:**
- PTQ's affine mapping quantizes/dequantizes via a scale and zero-point set from a tensor's
  observed range; outliers inflate the scale and degrade precision for the rest of the tensor —
  per-channel quantization mitigates this.
- QAT simulates quantization error during training via a straight-through estimator through the
  non-differentiable round operation, typically recovering most of PTQ's accuracy gap.
- Knowledge distillation trains a student to match a temperature-softened teacher distribution
  (plus hard labels); the $T^2$ rescaling keeps the two loss terms' gradients comparable.
- Weeks 1–8 derivations (SDE diffusion, classifier-free guidance, flow matching, MoE, ICL
  mechanics, Bradley-Terry/RLHF, PPO/DPO, quantization, distillation) are midterm scope.

**You should now be able to:** derive and implement PTQ and QAT; derive and implement the
distillation loss; recall and re-derive Weeks 1–8's central results for the midterm.

**Next week:** Midterm Exam, followed by Efficient Inference II — speculative decoding's
accept/reject correctness argument and KV-cache management.
