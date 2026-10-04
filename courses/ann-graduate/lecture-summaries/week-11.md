# Week 11 Summary — Expressivity and Depth

**Key takeaways:**
- A residual block $y=x+F(x)$ has Jacobian $I+\partial F/\partial x$; if $F$ starts near zero,
  this stays close to $I$, so gradients survive composition through many stacked blocks far
  better than through plain blocks, whose Jacobian product can vanish geometrically with depth.
- This is an optimization-theory argument about trainability near initialization, not a
  statement about the ResNet architecture in full (owned by the Deep Learning course).
- Attention gives every pair of positions an $O(1)$-length information path in one layer, a
  structurally different expressivity profile from sequential/local computation.

**You should now be able to:** explain, via the Jacobian argument, why skip connections ease
optimization in very deep networks, and state the expressivity argument for attention briefly.

**Next week:** the Neural Tangent Kernel — infinite-width networks as kernel regression.
