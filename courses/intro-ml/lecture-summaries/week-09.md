# Week 9 Summary — Midterm Exam + Support Vector Machines

**Key takeaways:**
- SVMs find the maximum-margin hyperplane separating two classes; only the support vectors
  determine the boundary.
- Soft margins (controlled by `C`) tolerate some margin violations, trading margin width for
  training fit.
- The kernel trick (linear, polynomial, RBF) computes inner products in a higher-dimensional
  space implicitly, enabling non-linear decision boundaries without explicitly transforming
  features.
- SVMs require feature scaling, since margins and kernels depend directly on distances between
  points.

**You should now be able to:** fit `SVC` with different kernels; explain the role of `C` and
`gamma`; choose a kernel appropriate to a dataset's apparent separability.

**Reminder:** the Capstone project was introduced this week; the proposal is due in Week 10.
**Next week:** model evaluation and selection in depth — cross-validation, grid/random search,
and learning curves.
