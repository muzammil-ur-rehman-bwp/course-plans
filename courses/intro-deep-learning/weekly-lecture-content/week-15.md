# Week 15 — Lecture Content: Ethics and Current Trends

## 1. Bias Amplification in Deep Models

A model trained on data reflecting existing societal or sampling disparities does not merely
reproduce those disparities — it can **amplify** them. Two common mechanisms:

- **Skewed training data:** if a group is underrepresented, the model's loss is dominated by the
  majority group, and the model's effective accuracy on the minority group can be substantially
  worse, even though *overall* accuracy looks acceptable.
- **Proxy features:** a feature correlated with a protected attribute (e.g., zip code correlating
  with race, or certain words correlating with gender) can let a model reconstruct and act on that
  attribute even when it is never provided directly, and because the correlation in training data
  can be stronger than the real-world relationship, the model's reliance on it can overstate the
  pattern at deployment time.

**Worked case study:** an image classifier trained predominantly on images from one demographic
group shows a measurably higher error rate on a different demographic group at evaluation time.
The likely cause is not a flaw in the architecture (the same CNN/ResNet studied in Weeks 3–5) but
in the training data's composition — the fix is in data collection and evaluation practice (e.g.,
reporting subgroup-level metrics, not just aggregate accuracy), not in a different loss function.

## 2. The Compute and Environmental Cost of Large-Scale Training

Training cost scales roughly with the number of parameters multiplied by the number of training
steps and the data processed per step (total training FLOPs). As models and datasets have grown,
training cost has grown correspondingly, with real energy consumption and associated carbon
footprint. This is a genuine engineering and ethical trade-off: a marginally more accurate model
trained at many times the compute cost may not be the responsible choice for a given application.

```python
# A simple, illustrative relative-cost estimate (not a precise FLOPs count):
def relative_training_cost(num_params, num_steps, batch_size):
    return num_params * num_steps * batch_size   # proportional to total compute, not an exact FLOPs count

cost_a = relative_training_cost(num_params=11_000_000, num_steps=50_000, batch_size=64)   # e.g., ResNet-18-scale
cost_b = relative_training_cost(num_params=300_000_000, num_steps=200_000, batch_size=256)  # a much larger model
print(cost_b / cost_a)   # a rough multiplier of how much more compute model B required
```

## 3. Current Trends, Grounded

- **Foundation models:** large models pretrained on broad data and adapted to many downstream
  tasks. This is transfer learning (Week 5) taken to a larger scale — the underlying mechanism
  (reuse pretrained features, fine-tune or adapt for a new task) is the same one already studied,
  not a fundamentally new idea.
- **Self-supervised pretraining:** learning useful representations from unlabeled data by solving
  an auxiliary task defined from the data itself (e.g., predicting a masked portion of the input).
  This connects directly to the autoencoder's unsupervised reconstruction objective (Week 10) and
  to the Transformer architecture (Week 9), which many self-supervised pretraining schemes use as
  their backbone.

Presenting these as natural extensions of material already covered, rather than as unexplained new
breakthroughs, is deliberate: understanding *why* they work follows directly from this semester's
content.

## 4. In-Class Exercise

Given two model configurations' parameter counts and training step counts, compute the relative
training-cost estimate above, and discuss one practical implication of a large cost difference
(e.g., for an organization with limited compute budget, or for environmental impact reporting).
