# Week 5 Summary — Feature Learning Beyond the Kernel Regime

**Key takeaways:**
- Finite width, parameterization choice, and learning-rate/training-horizon effects each push a
  practical network away from the Week 2 lazy-training idealization.
- Kernel drift, hidden-representation task-alignment, and the kernel-regression-vs-trained-network
  performance gap are three concrete, measurable signatures of feature learning.
- Feature learning is widely believed central to deep learning's practical success in a way NTK
  theory, by its own construction, cannot address — the two are separate theoretical objects, not
  one a refinement of the other.

**You should now be able to:** name three mechanisms that break the lazy-training approximation at
practical widths; measure and interpret kernel drift and the kernel-regression-vs-trained-network
gap; explain why feature learning is treated as a distinct open question from NTK trainability.

**Next week:** Sharpness and generalization — flat vs. sharp minima, the debated reliability of the
sharpness-generalization correlation, and Sharpness-Aware Minimization (SAM).
