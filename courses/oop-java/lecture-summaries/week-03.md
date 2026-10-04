# Week 3 Summary — Composition ("Has-A" Relationships)

**Key takeaways:**
- An object can hold another object as a field and delegate to its public interface rather than
  duplicating its logic — this is composition, the "has-a" relationship.
- A composed object can be constructed internally by the enclosing class or passed in already
  built; passing it in is more flexible and becomes more useful once interfaces (Week 9) make the
  composed type swappable.
- A class should be composed of other classes it genuinely *has*, not forced into an inheritance
  relationship it does not actually have.

**You should now be able to:** design a class built from one or more composed objects, with each
class responsible only for its own fields and behavior.

**Next week:** static vs. instance members — static fields/methods, static initialization, and
utility classes.
