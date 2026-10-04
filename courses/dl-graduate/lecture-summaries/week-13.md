# Week 13 Summary — Multimodal and Foundation Models (Grounded Survey)

**Key takeaways:**
- Vision-language models learn a joint embedding space via a CLIP-style contrastive objective —
  Week 5's InfoNCE framing, extended across the image and text modalities.
- Foundation models are pretrained broadly, then adapted via full fine-tuning or Week 12's
  parameter-efficient methods.
- Pretraining compute scales roughly with parameters × training tokens; the largest-scale
  pretraining runs require resources far beyond most labs, and data bias/quality are active,
  unresolved concerns.

**You should now be able to:** describe the CLIP-style contrastive pretraining objective,
estimate relative pretraining compute from parameter/data scale, and discuss one concrete
limitation of a current multimodal model.

**Next week:** research methods in deep learning — reading/critiquing papers, DL-specific
reproducibility challenges, and capstone work time.
