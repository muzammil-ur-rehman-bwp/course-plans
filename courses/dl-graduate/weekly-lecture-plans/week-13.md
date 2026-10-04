# Week 13 Lecture Plan — Deep Learning (Graduate)
## Topic: Multimodal and Foundation Models (Grounded Survey)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain CLIP-style contrastive vision-language pretraining as an extension of Week 5's
   InfoNCE framing. (*Understand*)
2. Evaluate the pretrain-then-finetune/adapt paradigm at scale, including compute/data costs.
   (*Evaluate*)
3. Critique one concrete limitation of a current multimodal/foundation model. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 5's single-modality contrastive learning; today: across two modalities |
| 0:15–0:45 | Vision-language models | Joint embedding spaces; CLIP-style contrastive image-text pretraining |
| 0:45–1:05 | Pretrain-then-adapt at scale | Foundation models; fine-tuning vs. Week 12's parameter-efficient adaptation |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Compute/data cost, honestly | Parameter count × data scale estimates; who can train these models |
| 1:40–2:00 | Limitations discussion | Data bias/quality, evaluation difficulty; structured case-study discussion |

### Materials/Equipment
- Slide diagrams: joint embedding space illustration; a compute-cost estimation worksheet
- Case-study handout (instructor-provided, refreshed per term)

### Formative Check (in-class)
Given two example model configurations' parameter counts and training-token counts, students
estimate relative pretraining compute and discuss what that implies about who can train such
models.

### Link to Lab/Assessment
Lab 13: A short notebook exercise estimating pretraining compute from parameter/data scale, and
sketching a CLIP-style contrastive loss for paired image-text batches (see
`lab-manuals/lab-13.md`).
