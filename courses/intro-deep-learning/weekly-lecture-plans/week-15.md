# Week 15 Lecture Plan — Introduction to Deep Learning
## Topic: Ethics and Current Trends

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Evaluate a case study of bias amplification in a deep model and identify the likely cause.
   (*Evaluate*)
2. Evaluate the environmental/compute cost trade-offs of large-scale model training. (*Evaluate*)
3. Explain foundation models and self-supervised pretraining as extensions of transfer learning and
   the Transformer, without hype. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Bias amplification | How skewed training data and proxy features amplify disparities; a worked case study |
| 0:20–0:40 | Discussion | Small-group analysis of the case study's likely data/design cause |
| 0:40–0:50 | Break | — |
| 0:50–1:15 | Compute/environmental cost | FLOPs, energy use, growing training cost trends; estimating relative cost from parameter count and steps |
| 1:15–1:45 | Current trends, grounded | Foundation models and self-supervised pretraining as extensions of Weeks 5 and 9, not unexplained hype |
| 1:45–2:00 | Course-map preview | Setting up Week 16's full course recap |

### Materials/Equipment
- Case-study handout (bias amplification scenario)
- Worked compute-cost estimation example

### Formative Check (in-class)
Given two models' parameter counts and training step counts, estimate which required
substantially more training compute, and name one practical implication of that difference.

### Link to Lab/Assessment
Lab 15: Case study — analyze a provided dataset/model scenario for a subgroup performance gap, and
estimate relative training compute cost for two example model configurations.
