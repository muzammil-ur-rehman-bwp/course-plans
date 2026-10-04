# Week 15 — Lecture Content: Evaluating and Debugging Neural Networks

## 1. Four Reference Loss-Curve Patterns
| Pattern | Training loss | Validation loss | Diagnosis |
|---|---|---|---|
| Healthy | Decreasing, flattening | Decreasing, flattening, close to training | Good fit; consider stopping or minor further tuning |
| Underfitting | High, flat/slowly decreasing | High, close to training loss | Model too simple, undertrained, or learning rate too low |
| Overfitting | Low, still decreasing | Decreasing then rising (widening gap) | Excess capacity; needs regularization (Week 11) or more data |
| Broken | Flat, high, or `nan`/erratic | Same | Bug or severe hyperparameter misconfiguration |

This table operationalizes the overfitting diagnosis first introduced in Week 11 and generalizes
it to cover the full space of training outcomes across every architecture this semester has
covered (MLP, CNN, RNN).

## 2. Diagnosing a Broken Training Loop
If training loss does not decrease at all (or immediately becomes `nan`), check, in this order
(cheapest checks first):
1. **Labels**: are they correctly aligned with inputs? A shuffled mismatch between `X` and `y`
   produces a loss that looks like random-guessing performance and never improves.
2. **Normalization**: are inputs scaled reasonably (e.g., pixels in $[0,1]$, not $[0,255]$)?
   Unnormalized inputs can produce extremely large pre-activations, causing `nan` losses or
   extremely slow learning — the same input-scaling issue flagged in Week 6's lab notes.
3. **Learning rate**: too high causes divergence/`nan`; too low looks like "stuck, flat" loss
   that never moves — these can look similar at a glance, so try both a 10× smaller and a 10×
   larger learning rate as a quick diagnostic.
4. **Loss/activation pairing**: is the loss function paired with the correct output activation
   (Week 5)? A mismatched pairing (e.g., `nn.CrossEntropyLoss` applied to an output that has
   already had softmax applied) silently gives wrong gradients without necessarily crashing.
5. **A gradient check** (Week 7), if a from-scratch component is involved, isolates whether the
   backward pass itself is correct before suspecting the data or hyperparameters further.

## 3. Hyperparameter Tuning Basics
Key hyperparameters and their typical effect:
| Hyperparameter | Effect of increasing it |
|---|---|
| Learning rate | Faster convergence, up to a point; too high diverges |
| Batch size | Smoother/more stable gradient estimates; may need a proportionally adjusted learning rate |
| Hidden width/depth | More capacity (can fit more complex functions); more overfitting risk without regularization |
| Regularization strength ($\lambda$, dropout $p$) | Less overfitting, but underfitting if too strong |

A simple, systematic approach is a **grid search**: define a small set of candidate values for
each hyperparameter, train a model for every combination, and select the combination with the
best *validation* performance (never the test set, which must stay untouched until final
reporting — the Week 9/12 train/validation/test discipline applies here too).

```python
import itertools

learning_rates = [0.1, 0.01, 0.001]
hidden_sizes = [32, 64, 128]
best_val_loss, best_config = float("inf"), None

for lr, hidden_size in itertools.product(learning_rates, hidden_sizes):
    val_loss = train_and_evaluate(lr=lr, hidden_size=hidden_size)   # returns best validation loss
    if val_loss < best_val_loss:
        best_val_loss, best_config = val_loss, (lr, hidden_size)

print("Best config:", best_config, "validation loss:", best_val_loss)
```

## 4. In-Class Exercise
Given four unlabeled loss-curve plots (healthy, underfitting, overfitting, broken — provided as a
handout), match each to its diagnosis from the table above, and state the single most likely next
action for each (e.g., "add dropout," "increase learning rate," "check label alignment").
