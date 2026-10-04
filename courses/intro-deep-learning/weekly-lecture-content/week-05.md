# Week 5 — Lecture Content: Training Deep Networks at Scale

## 1. Data Augmentation

Augmentation applies label-preserving random transformations to training images, exposing the
model to variation it would otherwise not see and reducing overfitting:

```python
import torchvision.transforms as T

train_transform = T.Compose([
    T.RandomCrop(32, padding=4),
    T.RandomHorizontalFlip(),
    T.ColorJitter(brightness=0.2, contrast=0.2),
    T.ToTensor(),
])
```

Augmentation is applied only to the training set — validation/test transforms should be
deterministic (resize/center-crop + `ToTensor()` only), so evaluation measures the model, not the
augmentation.

## 2. Learning Rate Schedules and Warmup

A fixed learning rate is rarely optimal for the whole run. Common schedules:

```python
import torch.optim as optim

optimizer = optim.SGD(model.parameters(), lr=0.1, momentum=0.9)

step_scheduler = optim.lr_scheduler.StepLR(optimizer, step_size=30, gamma=0.1)        # decay by 10x every 30 epochs
cosine_scheduler = optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=num_epochs)   # smooth decay to ~0

for epoch in range(num_epochs):
    for xb, yb in train_loader:
        ...  # standard training step
    cosine_scheduler.step()
```

**Warmup** linearly ramps the learning rate up from a small value over the first few epochs/steps
before the main schedule takes over. Early in training, parameters are far from any good
solution and gradients can be large or noisy; a large learning rate applied immediately can cause
instability. Warmup is especially important with large batch sizes (Week 13) and large learning
rates.

```python
def warmup_lr(step, warmup_steps, base_lr):
    return base_lr * min(1.0, step / warmup_steps)
```

## 3. Transfer Learning and Fine-Tuning

A model pretrained on a large dataset (e.g., ImageNet) has already learned general-purpose visual
features in its early/middle layers. Transfer learning reuses those features for a new task:

- **Feature extraction:** freeze the pretrained backbone, train only a new final layer. Suits
  small target datasets, where fine-tuning the whole network would overfit.
- **Fine-tuning:** unfreeze some or all of the backbone and continue training (usually with a
  smaller learning rate), adapting the learned features to the new task. Suits larger target
  datasets.

```python
import torchvision.models as models
import torch.nn as nn

model = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)

# Feature extraction: freeze everything except the new head
for param in model.parameters():
    param.requires_grad = False

num_classes = 10
model.fc = nn.Linear(model.fc.in_features, num_classes)   # new head, trainable by default

optimizer = optim.Adam(model.fc.parameters(), lr=1e-3)     # only the new head's parameters
```

```python
# Fine-tuning: unfreeze the last residual stage as well, with a smaller LR for pretrained layers
for param in model.layer4.parameters():
    param.requires_grad = True

optimizer = optim.Adam([
    {"params": model.layer4.parameters(), "lr": 1e-4},
    {"params": model.fc.parameters(), "lr": 1e-3},
])
```

## 4. Putting It Together

```python
for epoch in range(num_epochs):
    model.train()
    for xb, yb in train_loader:
        optimizer.zero_grad()
        loss = criterion(model(xb), yb)
        loss.backward()
        optimizer.step()
    cosine_scheduler.step()

    model.eval()
    # ... compute validation loss/accuracy with model.eval() (affects BatchNorm/Dropout) ...
```

## 5. In-Class Exercise

Given a target dataset of only 200 labeled images, decide between feature extraction and
fine-tuning, and justify the choice in terms of overfitting risk.
