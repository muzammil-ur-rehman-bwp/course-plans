# Presentation: Module 4 — Scaling, Multimodal Models, Research Methods & Capstone (Weeks 12–16)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 4: Scaling, Multimodal Models, Research Methods & Capstone
2. **Data vs. model parallelism** — batch-splitting vs. layer/parameter-splitting
3. **Mixed-precision training** — FP16/BF16; loss scaling and why it's needed
4. **Gradient checkpointing** — recompute-vs-store trade-off
5. **LoRA** — freezing $W$, learning $BA$; parameter savings
6. **Vision-language models** — joint embedding spaces; CLIP-style contrastive pretraining
7. **Pretrain-then-adapt at scale** — foundation models; fine-tuning vs. parameter-efficient
   adaptation
8. **Compute/data costs, honestly** — rough FLOP estimation; who can train at this scale
9. **Reading and critiquing a DL paper** — the structured checklist
10. **DL-specific reproducibility challenges** — compute cost, hyperparameter sensitivity,
    benchmark culture
11. **The research capstone** — literature review, experiment, paper, presentation structure
12. **Course map recap** — the full semester's architecture-and-technique landscape
13. **Connecting to sibling graduate courses** — ANN theory, AI/tabular RL, ML statistical theory

**Speaker notes:** slide 8 (compute/data costs, honestly) sets the tone for the whole module —
this course's final stretch should leave students able to evaluate claims about scale critically,
not simply impressed by them; carry that critical framing directly into slide 10's
reproducibility discussion and the capstone itself.
