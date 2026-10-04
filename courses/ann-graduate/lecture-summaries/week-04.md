# Week 4 Summary — Initialization Theory

**Key takeaways:**
- All-zero (or all-equal) initialization fails via symmetry: every unit in a layer updates
  identically forever.
- Naive fixed-variance random initialization causes activation variance to grow or shrink
  geometrically with depth unless weight variance is scaled by $1/n_{in}$.
- Xavier/Glorot initialization ($\mathrm{Var}(W)=2/(n_{in}+n_{out})$) balances forward- and
  backward-variance preservation for tanh-like activations; He initialization
  ($\mathrm{Var}(W)=2/n_{in}$) corrects for ReLU halving variance each layer.

**You should now be able to:** derive both formulas from variance-preservation arguments and
explain why the activation function determines which one to use.

**Next week:** normalization theory — deriving Batch Normalization and Layer Normalization, and
critically comparing the two leading explanations for why BatchNorm works.
