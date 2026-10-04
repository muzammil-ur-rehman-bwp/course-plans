# Week 4 Summary — Transformer Variants and Applications

**Key takeaways:**
- Encoder-only models use full bidirectional attention and are pretrained via masked language
  modeling (predicting randomly masked tokens from context).
- Decoder-only models use a causal attention mask (blocking attention to future positions) and
  are pretrained via next-token prediction, generating autoregressively at inference.
- The Vision Transformer treats fixed-size image patches as tokens, linearly projects them, adds
  a class token and positional embeddings, and applies a standard Transformer encoder.

**You should now be able to:** implement a causal attention mask, implement a ViT-style
patch-embedding pipeline, and explain each variant's attention-masking pattern.

**Next week:** self-supervised and contrastive representation learning — pretext tasks, the
InfoNCE loss, and linear probing.
