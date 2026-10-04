# Week 1 Summary — The Statistical Learning Framework

**Key takeaways:**
- True risk $L_D(h)$ is what we care about; empirical risk $\widehat L_S(h)$ is what we can
  compute; ERM minimizes the latter as a proxy for the former.
- Unrestricted ERM (over "all functions") can achieve zero empirical risk while generalizing no
  better than chance — overfitting is a provable fact about unrestricted hypothesis classes, not
  just a practical symptom.
- This course builds the theory behind the applied algorithms from *Introduction to Machine
  Learning*, and is explicitly scoped apart from ANN-Graduate, AI-Graduate, and DL-Graduate.

**You should now be able to:** state risk, empirical risk, and the ERM principle precisely; argue
why an unrestricted hypothesis class cannot generalize from empirical risk alone.

**Next week:** PAC learning — making "restricted enough to generalize" precise via sample
complexity, derived from Hoeffding's inequality and a union bound.
