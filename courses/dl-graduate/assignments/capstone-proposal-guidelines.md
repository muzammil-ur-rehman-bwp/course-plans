# Research Capstone — Proposal Guidelines

**Topic selection opens:** Week 7–8 (informal discussion with instructor) | **Formal proposal
due:** End of Week 9 | **Weight:** part of the Research Capstone (25% of course grade total)

## What to Submit
A 1–2 page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Subtopic:** which deep-learning subtopic from this course (or closely adjacent, with
   instructor approval) the project addresses — e.g., advanced CNN architectures, the Transformer
   and its variants, self-supervised/contrastive learning, normalizing flows or diffusion models,
   graph neural networks, deep reinforcement learning, large-scale training practices, or
   multimodal/foundation models.
2. **Motivating question:** what specific question or gap is the project investigating? (This need
   not be fully refined yet — Week 15's work session will sharpen it — but it must be more
   specific than "I want to study diffusion models.")
3. **Planned literature:** 3–5 candidate papers (title/authors/venue/year, as best known) the
   student plans to review; these may change as the literature review develops, but the proposal
   must show a genuine starting point, not a placeholder.
4. **Planned experiment or derivation:** a reproduced or extended experiment from one of the
   candidate papers, stated concretely enough that a feasible scope is clear (e.g., "compare GCN
   vs. GAT node-classification accuracy on the same small citation graph across 3 seeds" is
   concrete; "study graph neural networks" is not).
5. **Team:** individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
build on techniques covered in this course; topics requiring deep theoretical content
(backpropagation-as-reverse-mode-AD, initialization/normalization/loss-landscape theory,
generalization theory) or classical statistical ML theory — owned by the sibling graduate
*Artificial Neural Network* and *Machine Learning* courses — require instructor pre-approval and
must still center on this course's own architecture/technique focus.

## Example Topics (for inspiration, not a closed list)
A controlled comparison of ResNet vs. DenseNet parameter efficiency at matched depth; a
small-scale reproduction of SimCLR-style linear-probe accuracy versus pretraining epoch count; a
normalizing-flow vs. diffusion-model comparison on 2-D synthetic data; an empirical study of GCN
vs. GraphSAGE vs. GAT on a small node-classification benchmark; a DQN ablation isolating the
effect of experience replay buffer size; a REINFORCE-with-baseline vs. plain-REINFORCE variance
comparison on a toy environment; a small LoRA-rank-vs-downstream-accuracy sweep on a fine-tuning
task.
