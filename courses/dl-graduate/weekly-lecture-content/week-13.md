# Week 13 — Lecture Content: Multimodal and Foundation Models (Grounded Survey)

## 1. Vision-Language Models: Joint Embedding via Contrastive Pretraining

A vision-language model's core idea is to learn a **shared embedding space** in which an image
and a caption that describe the same thing end up close together, and unrelated image-text pairs
end up far apart. This is a direct two-modality extension of Week 5's contrastive framing: instead
of two augmented views of the *same* image, the positive pair is an (image, matching caption)
pair, encoded by two **separate** encoders (an image encoder $f_\theta$, a text encoder $g_\phi$)
into a shared-dimensional embedding space.

A CLIP-style contrastive objective, for a batch of $N$ (image, text) pairs, treats it as a
bidirectional retrieval problem: for each image, identify its matching text among the $N$ texts in
the batch (and symmetrically, for each text, identify its matching image), using exactly the same
softmax-over-similarities InfoNCE-style loss from Week 5, applied **across modalities**:

$$
\mathcal{L} = \frac{1}{2}\Big(\underbrace{\frac{1}{N}\sum_{i=1}^N -\log
\frac{\exp(\text{sim}(I_i,T_i)/\tau)}{\sum_{j=1}^N \exp(\text{sim}(I_i,T_j)/\tau)}}_{\text{image}
\to\text{text}} \ +\ \underbrace{\frac{1}{N}\sum_{i=1}^N -\log
\frac{\exp(\text{sim}(T_i,I_i)/\tau)}{\sum_{j=1}^N \exp(\text{sim}(T_i,I_j)/\tau)}}_{\text{text}
\to\text{image}}\Big).
$$

```python
import torch
import torch.nn.functional as F

def clip_style_loss(image_embeds, text_embeds, temperature=0.07):
    image_embeds = F.normalize(image_embeds, dim=1)
    text_embeds = F.normalize(text_embeds, dim=1)
    logits = image_embeds @ text_embeds.T / temperature         # (N, N)
    targets = torch.arange(logits.size(0), device=logits.device)
    loss_i2t = F.cross_entropy(logits, targets)
    loss_t2i = F.cross_entropy(logits.T, targets)
    return (loss_i2t + loss_t2i) / 2
```

After pretraining on large-scale (image, text) pairs, the resulting joint embedding space supports
zero-shot classification (compare an image's embedding to the embeddings of several candidate
textual class descriptions and pick the closest) without any task-specific labeled fine-tuning —
the headline capability that made this approach influential.

## 2. The Pretrain-Then-Adapt Paradigm at Scale

A **foundation model** is a large model pretrained once on broad data (often self-supervised or
contrastive, as above) and then **adapted** to many downstream tasks, either by:
- full fine-tuning (updating all parameters on task-specific labeled data), or
- the parameter-efficient methods from Week 12 (e.g., LoRA), which adapt a large pretrained model
  to a new task while updating only a small fraction of its parameters — increasingly the default
  at the scale foundation models operate at, since full fine-tuning of a very large pretrained
  model is often prohibitively expensive to do per-task.

This is the same pretrain-then-transfer pattern already familiar from the undergraduate course's
`torchvision.models`-based transfer learning, scaled up and generalized across modalities.

## 3. Compute and Data Requirements, Honestly

Pretraining compute for a transformer-style model scales roughly with (parameter count) ×
(training tokens/samples processed), and both have grown enormously across successive
foundation-model generations. A grounded (non-hype) accounting must be explicit that:
- Training runs at the largest scales require specialized hardware clusters and energy budgets
  well beyond a typical university or small-company lab — a genuine barrier to who can train
  (as opposed to fine-tune or use) such models.
- Data scale and quality both matter: a larger but lower-quality/more-biased dataset does not
  straightforwardly produce a better or safer model, and large web-scraped datasets carry known
  bias, copyright, and consent concerns that are active, unresolved areas of concern — not a
  solved problem to wave away.

```python
def rough_pretraining_compute(num_params, num_training_tokens):
    """A standard rough estimate: ~6 FLOPs per parameter per training token
    (forward + backward pass), used only for order-of-magnitude comparison."""
    return 6 * num_params * num_training_tokens

model_a = rough_pretraining_compute(num_params=1e8, num_training_tokens=1e10)
model_b = rough_pretraining_compute(num_params=1e10, num_training_tokens=3e11)
print(f"Model A: {model_a:.2e} FLOPs   Model B: {model_b:.2e} FLOPs   "
      f"ratio: {model_b/model_a:,.0f}x")
```

## 4. Limitations, Discussed Honestly

- **Evaluation difficulty:** benchmark performance for multimodal/foundation models can
  overstate real-world robustness; the same benchmark-culture critique from Week 14's
  reproducibility discussion applies directly.
- **Data bias and quality:** large web-scraped pretraining corpora reflect and can amplify
  existing societal biases and are unevenly representative across languages/cultures/demographics.
- **Compute concentration:** the ability to pretrain (not merely fine-tune or deploy) models at
  the largest scales is concentrated among a small number of well-resourced organizations, with
  consequences for who can do foundational research versus who can only build on top of it.

## 5. In-Class Exercise

Using the rough compute formula above, estimate the relative pretraining compute of a 50M-parameter
model trained on 2B tokens versus a 7B-parameter model trained on 1T tokens, and discuss in 2–3
sentences what the ratio implies about who can feasibly run each kind of training.
