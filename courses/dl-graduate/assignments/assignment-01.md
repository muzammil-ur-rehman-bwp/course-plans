# Assignment 1 — Advanced CNNs and the Transformer From Scratch (Weeks 2–3)

**Weight:** 5% of course grade (one of 3 problem-set assignments, 15% total) | **Assigned:** Week
4 | **Due:** Start of Week 6

## Instructions
Submit a single Jupyter notebook `assignment01.ipynb` answering all questions below. Show your
work (derivations and/or code) for each question.

## Questions
1. **(Residual formulation, 15 pts)** Starting from $H(x) = F(x) + x$, derive
   $\partial H/\partial x$ and explain, in 3–4 sentences, why this gives gradients a direct path
   back through a stack of residual blocks, referencing the identity-mapping argument from Week 2.
2. **(Efficiency architectures, 20 pts)** For $C_{in}=48$, $C_{out}=96$, $k=3$, compute the
   standard-convolution and depthwise-separable multiply-add counts by hand; implement both in
   PyTorch and confirm your hand-computed parameter counts match `sum(p.numel() for p in
   model.parameters())`.
3. **(Scaled dot-product attention, 20 pts)** For a toy $Q,K$ with $d_k=8$ and a query/key pair
   whose unscaled dot product is $24$, compute the scaled score and compare the resulting softmax
   weight (against one other candidate key with unscaled score $6$) to the unscaled case;
   explain the softmax-saturation argument from lecture using your numbers.
4. **(Multi-head attention implementation, 25 pts)** Implement multi-head attention from scratch
   (no `nn.MultiheadAttention`) for $d_{model}=64$, $h=8$; verify output shape is preserved for a
   batch of sequences of two different lengths, and verify that using $h=1$ head reduces to
   standard (single-head) scaled dot-product attention on the same inputs (numerically matching
   outputs).
5. **(Positional encoding, 20 pts)** Implement sinusoidal positional encoding for $d_{model}=16$;
   numerically verify the relative-position property for 3 different $(pos, k)$ offset pairs, and
   explain in 3–4 sentences why this property matters for a model that must generalize to
   sequence lengths not seen during training.

## Submission
Upload `assignment01.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
