# Week 7 Lecture Plan — Machine Learning (Graduate)
## Topic: Kernel Methods and RKHS Theory

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the positive-definite kernel condition and Mercer's theorem conceptually. (*Understand*)
2. Construct the RKHS for a kernel via the reproducing property. (*Understand, Apply*)
3. State and prove (sketch) the representer theorem. (*Apply, Analyze*)
4. Connect the representer theorem rigorously back to kernel SVMs and kernel ridge regression. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Positive-definite kernels | Gram-matrix PSD condition; Mercer's theorem (conceptual) |
| 0:20–0:45 | Constructing the RKHS | The reproducing property, derived from the inner-product definition |
| 0:45–1:10 | Representer theorem | Full proof sketch via orthogonal decomposition and Pythagoras |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Why it matters | Finite-dimensional reduction; connection to kernel SVM dual (Week 6) |
| 1:45–2:00 | Kernel ridge regression | Deriving the closed-form finite-dimensional solve |

### Materials/Equipment
- Whiteboard for the representer-theorem proof
- Jupyter notebook comparing from-scratch kernel ridge regression to `sklearn.kernel_ridge`

### Formative Check (in-class)
Students explain, in their own words, why $f_\perp$ (the orthogonal component) is invisible to
the data-fit term but not to the regularization term, and why that is exactly what forces
$f_\perp=0$ at the optimum.

### Link to Lab/Assessment
Lab 7: implement kernel ridge regression from scratch via the representer theorem; cross-check
against `sklearn.kernel_ridge.KernelRidge`.
