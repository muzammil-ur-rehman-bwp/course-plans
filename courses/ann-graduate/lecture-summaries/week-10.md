# Week 10 Summary — Generalization Theory II

**Key takeaways:**
- Rademacher complexity measures a class's capacity to fit random labels on the *actual* data
  distribution, often giving tighter, more informative bounds than distribution-free VC dimension.
- Margin-based arguments let a large-margin solution generalize better than its raw capacity would
  suggest, by effectively shrinking the complexity measure that enters the bound.
- Networks that fit entirely random labels to zero training error can still generalize normally
  when trained on real labels — proving capacity alone cannot be what controls generalization;
  the explanation must lie in what training actually finds (implicit bias), not in what the
  hypothesis class could in principle represent.

**You should now be able to:** explain Rademacher complexity and margin bounds conceptually, and
explain precisely why the random-label-fitting result does not contradict good generalization on
real data.

**Next week:** expressivity and depth — the optimization-theory case for skip connections and the
expressivity case for attention, both brief and theory-angled only.
