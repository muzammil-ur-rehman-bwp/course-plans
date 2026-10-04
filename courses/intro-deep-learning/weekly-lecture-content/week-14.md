# Week 14 — Lecture Content: Practical Deep Learning Workflows

## 1. An End-to-End Transfer-Learning Workflow

This week assembles components from every prior week into one repeatable pipeline:

```python
import torch
import torch.nn as nn
import torch.optim as optim
import torchvision.models as models
import torchvision.transforms as T
import torchvision.datasets as datasets

# 1. Data loading + augmentation (Week 5)
train_transform = T.Compose([T.RandomResizedCrop(224), T.RandomHorizontalFlip(), T.ToTensor()])
eval_transform = T.Compose([T.Resize(256), T.CenterCrop(224), T.ToTensor()])
train_set = datasets.ImageFolder("data/train", transform=train_transform)
val_set = datasets.ImageFolder("data/val", transform=eval_transform)
train_loader = torch.utils.data.DataLoader(train_set, batch_size=32, shuffle=True)
val_loader = torch.utils.data.DataLoader(val_set, batch_size=32)

# 2. Pretrained backbone + new head (Week 5)
model = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)
model.fc = nn.Linear(model.fc.in_features, len(train_set.classes))

# 3. Optimizer with decoupled weight decay (Week 13) + LR schedule with warmup (Week 5)
optimizer = optim.AdamW(model.parameters(), lr=1e-4, weight_decay=1e-4)
scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=20)
criterion = nn.CrossEntropyLoss(label_smoothing=0.1)   # Week 13

# 4. Training with correct train/eval mode switching (Week 2)
for epoch in range(20):
    model.train()
    for xb, yb in train_loader:
        optimizer.zero_grad()
        loss = criterion(model(xb), yb)
        loss.backward()
        optimizer.step()
    scheduler.step()

    model.eval()
    correct, total = 0, 0
    with torch.no_grad():
        for xb, yb in val_loader:
            preds = model(xb).argmax(dim=1)
            correct += (preds == yb).sum().item()
            total += yb.size(0)
    print(f"epoch {epoch}: val acc = {correct/total:.3f}")
```

## 2. Saving, Loading, and Exporting Models

```python
# Saving/loading a checkpoint (the standard, recommended PyTorch approach):
torch.save(model.state_dict(), "model_checkpoint.pt")

loaded_model = models.resnet18(weights=None)
loaded_model.fc = nn.Linear(loaded_model.fc.in_features, len(train_set.classes))
loaded_model.load_state_dict(torch.load("model_checkpoint.pt"))
loaded_model.eval()
```

For deployment beyond a Python training script, two conceptual export paths exist:
- **TorchScript** (`torch.jit.trace` or `torch.jit.script`) compiles the model into a
  serialized, framework-independent representation that can run without the original Python code.
- **ONNX export** (`torch.onnx.export`) converts the model to the Open Neural Network Exchange
  format, runnable by other inference engines. Both are mentioned here at a conceptual level;
  deployment-specific tooling is beyond this course's scope.

## 3. Debugging a Deep Network That Won't Train

A cheapest-check-first order, applying techniques from across the semester:

| Order | Check | What it catches |
|---|---|---|
| 1 | Inspect a few raw batches of data/labels | Mislabeled or misaligned data, wrong normalization |
| 2 | Try to overfit a single small batch | A model/loss/training-loop bug independent of data scale or regularization |
| 3 | Confirm `model.train()`/`model.eval()` are called correctly | BatchNorm/Dropout behaving unexpectedly (Week 2) |
| 4 | Inspect gradient norms (are they ~0, or exploding?) | Vanishing/exploding gradients (Weeks 2, 6) |
| 5 | Sanity-check the learning rate (try a much smaller one) | Divergence from too-large a learning rate |

```python
# Step 2 in code: can the model memorize one tiny batch?
xb, yb = next(iter(train_loader))
xb, yb = xb[:4], yb[:4]
for step in range(200):
    optimizer.zero_grad()
    loss = criterion(model(xb), yb)
    loss.backward()
    optimizer.step()
print(loss.item())   # should approach ~0 if the model/loop has no bug
```

```python
# Step 4 in code: inspect gradient norms after a backward pass
total_norm = sum(p.grad.norm(2).item() ** 2 for p in model.parameters() if p.grad is not None) ** 0.5
print(total_norm)
```

## 4. In-Class Exercise

Given a provided training script whose loss never decreases, apply the checklist above in order
and report which step revealed the fault.
