# Week 4 — Lecture Content: Transformer Variants and Applications

## 1. Encoder-Only Pretraining: BERT-Style Masked Language Modeling

An encoder-only model has no causal restriction: every token may attend to every other token,
including ones that come later in the sequence (full bidirectional context). This makes
next-token prediction trivial (the answer is already visible), so encoder-only models are instead
pretrained with **masked language modeling (MLM)**: a random subset (e.g., 15%) of input tokens is
replaced with a special `[MASK]` token (or a random/unchanged token, per the original recipe), and
the model is trained to predict the original token at each masked position from its full
bidirectional context:

$$
\mathcal{L}_{\text{MLM}} = -\sum_{i \in \text{masked}} \log p_\theta(x_i \mid x_{\setminus
\text{masked}}).
$$

Because every masked position's prediction can use context from *both* directions, the learned
representations are well suited to downstream understanding tasks (classification, extraction)
after fine-tuning.

```python
import torch

def apply_mlm_masking(input_ids, mask_token_id, vocab_size, mask_prob=0.15):
    labels = input_ids.clone()
    probs = torch.rand(input_ids.shape)
    mask_positions = probs < mask_prob
    masked_input = input_ids.clone()
    masked_input[mask_positions] = mask_token_id
    labels[~mask_positions] = -100          # ignore_index for unmasked positions in the loss
    return masked_input, labels
```

## 2. Decoder-Only Pretraining: GPT-Style Causal Language Modeling

A decoder-only model is trained for **next-token prediction**: $p_\theta(x_t \mid x_{<t})$. For
this objective to be well-posed, position $t$ must be prevented from attending to any position
$>t$ (otherwise the model could "see the answer"). This is enforced with a **causal mask**: before
the softmax in scaled dot-product attention, every score $(i,j)$ with $j>i$ is set to $-\infty$,
so after softmax those positions receive exactly zero attention weight.

$$
\text{score}_{ij} = \begin{cases}\dfrac{q_i\cdot k_j}{\sqrt{d_k}} & j \le i \\ -\infty & j>i
\end{cases}
$$

```python
import torch

def causal_mask(seq_len):
    # mask[i, j] = 1 if position i may attend to position j (j <= i), else 0
    return torch.tril(torch.ones(seq_len, seq_len)).bool()

mask = causal_mask(5)
print(mask)
# tensor([[ True, False, False, False, False],
#         [ True,  True, False, False, False],
#         [ True,  True,  True, False, False],
#         [ True,  True,  True,  True, False],
#         [ True,  True,  True,  True,  True]])
```

At inference, generation is autoregressive: sample/pick $x_t$ from $p_\theta(\cdot\mid x_{<t})$,
append it, and repeat — exactly the decoding pattern already familiar from the undergraduate
course's sequence-to-sequence models, now driven by self-attention instead of a recurrent hidden
state.

## 3. Vision Transformers (ViT): Patches as Tokens

A ViT applies the standard Transformer encoder to images by first converting an image into a
sequence of tokens:

1. **Patchify:** split a $H\times W\times C$ image into $N = \frac{HW}{P^2}$ non-overlapping
   $P\times P$ patches.
2. **Linearly project** each flattened patch ($P^2 C$ values) into a $d_{model}$-dimensional patch
   embedding — this plays the same role a word/token embedding plays in NLP.
3. **Prepend a learnable `[CLS]` token** whose final-layer representation is used for
   classification (analogous to BERT's `[CLS]`).
4. **Add positional embeddings** (learned, or the Week 3 sinusoidal form) since patch order
   information would otherwise be lost, exactly as in text.
5. Feed the resulting sequence through a standard Transformer encoder (Week 3); classify from the
   `[CLS]` token's final representation.

```python
import torch
import torch.nn as nn

class PatchEmbedding(nn.Module):
    def __init__(self, img_size=32, patch_size=4, in_channels=3, d_model=128):
        super().__init__()
        assert img_size % patch_size == 0
        self.num_patches = (img_size // patch_size) ** 2
        self.proj = nn.Conv2d(in_channels, d_model, kernel_size=patch_size, stride=patch_size)
        self.cls_token = nn.Parameter(torch.zeros(1, 1, d_model))
        self.pos_embed = nn.Parameter(torch.zeros(1, self.num_patches + 1, d_model))
        nn.init.trunc_normal_(self.pos_embed, std=0.02)

    def forward(self, x):
        B = x.size(0)
        patches = self.proj(x)                                  # (B, d_model, H/P, W/P)
        patches = patches.flatten(2).transpose(1, 2)             # (B, num_patches, d_model)
        cls = self.cls_token.expand(B, -1, -1)                   # (B, 1, d_model)
        tokens = torch.cat([cls, patches], dim=1)                # (B, num_patches+1, d_model)
        return tokens + self.pos_embed
```

Note that the `Conv2d` with `kernel_size=stride=patch_size` is exactly equivalent to "extract
each non-overlapping patch and apply the same linear projection to each" — a standard
implementation trick, not a different operation.

## 4. In-Class Exercise

For a 5-token sequence, draw the full attention-mask matrix (which $(i,j)$ pairs have nonzero
attention weight) for (a) an encoder-only model, (b) a decoder-only causal model, and (c) a
decoder's cross-attention over a 4-token encoder output. State one task each pattern is best
suited to.
