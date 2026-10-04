# Week 1 Summary — Biological Inspiration, History, and Course Roadmap

**Key takeaways:**
- Artificial neurons are only loosely inspired by biological neurons; the useful abstraction is a
  unit that combines weighted inputs and produces an output based on the combination's magnitude.
- Neural network history includes a major stall (the 1969–1986 "AI winter" for the field) caused
  by the perceptron's proven limitations and the lack of a multi-layer training algorithm, which
  backpropagation (1986) resolved.
- The McCulloch-Pitts neuron computes a weighted sum against a *fixed, hand-designed* threshold,
  and can realize basic logic gates (AND, OR, NOT) — but it does not learn.
- This course goes far beyond the brief two-week ANN survey in *Programming for AI*, building up
  to backpropagation, optimizers, regularization, frameworks, and basic architectures in depth.

**You should now be able to:** describe the biological-neuron analogy; place the field's key
historical milestones in order with justification; compute the output of a McCulloch-Pitts neuron
by hand and in code.

**Next week:** the perceptron — a neuron whose weights are *learned* from data — and why a single
perceptron cannot learn the XOR function.
