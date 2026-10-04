# Week 2 Summary — Constructors in Depth; Destructors

**Key takeaways:**
- Member initializer lists construct members directly; assignment in the constructor body
  default-constructs first, then overwrites — and reference/`const` members *require* an
  initializer list.
- A copy constructor (`T(const T&)`) runs on pass-by-value, return-by-value, and copy
  initialization; the compiler generates a shallow one if you don't write your own.
- A destructor (`~T()`) runs automatically at the end of an object's lifetime and is the place to
  release owned resources.
- The Rule of Three: a class that needs a custom destructor, copy constructor, or copy-assignment
  operator almost always needs all three, because a shallow compiler-generated copy of a raw
  owning pointer leads to double frees or dangling pointers.

**You should now be able to:** write constructors that use a member initializer list correctly,
write a destructor that releases a resource, write a deep-copying copy constructor, and explain
the Rule of Three.

**Next week:** operator overloading I — giving user-defined types natural arithmetic and
comparison syntax via member-function operators.
