# Week 7 Summary — Inheritance II

**Key takeaways:**
- Overriding redefines a base member function in a derived class with a matching signature;
  `override` tells the compiler to verify the match, turning signature-mismatch typos into
  compile errors.
- Name hiding (same name, different signature) hides *all* base overloads of that name from the
  derived class, unlike overriding — usually accidental, fixable with `using Base::name;`.
- C++ allows multiple inheritance (`class C : public A, public B`), useful for combining
  independent capabilities, but risky when the bases share a common ancestor.
- The diamond problem: two bases sharing an ancestor give the most-derived object two separate,
  ambiguous copies of that ancestor — recognize and generally avoid this shape rather than relying
  on `virtual` inheritance to patch it.

**You should now be able to:** override a member function correctly with `override`, distinguish
overriding from name hiding, and explain why diamond-shaped multiple inheritance is ambiguous.

**Next week:** polymorphism I — virtual functions, dynamic dispatch, virtual destructors, and
object slicing; plus midterm review.
