# Week 12 Lecture Plan — Machine Learning (Graduate)
## Topic: Dimensionality Reduction Theory

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Re-derive PCA as the variance-maximizing eigendecomposition. (*Apply, Analyze*)
2. Prove the optimality of the top-$k$ eigenvectors via the Courant–Fischer theorem (sketch). (*Analyze*)
3. Derive kernel PCA via the centered kernel matrix. (*Apply*)
4. Critically compare PCA, kernel PCA, Isomap, and t-SNE for a nonlinear dataset. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Variance-maximization formulation | Lagrangian argument; optimal directions are eigenvectors |
| 0:25–0:50 | Top-$k$ optimality | Courant–Fischer characterization; inductive deflation sketch |
| 0:50–1:10 | Kernel PCA | Centering the kernel matrix; projection formula |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Manifold learning survey | Isomap (geodesic distances); t-SNE (local-probability matching) |
| 1:45–2:00 | Comparative discussion | When linear PCA fails and nonlinear methods are needed |

### Materials/Equipment
- Whiteboard for the Lagrangian/eigenvector derivation
- Jupyter notebook: from-scratch PCA vs. `sklearn.decomposition.PCA`; kernel PCA on concentric circles

### Formative Check (in-class)
Students explain why the explained-variance ratio is non-decreasing in $k$ and reaches exactly 1
at $k=d$.

### Link to Lab/Assessment
Lab 12: implement PCA via eigendecomposition and kernel PCA from scratch; cross-check against
scikit-learn. **Assignment 3** assigned this week.
