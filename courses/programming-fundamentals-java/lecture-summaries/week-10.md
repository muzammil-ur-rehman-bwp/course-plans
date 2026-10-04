# Week 10 Summary — Using Existing Classes/APIs

**Key takeaways:**
- `ArrayList<T>` is a resizable, generic alternative to a fixed-size array, with `add`, `get`,
  `set`, `remove`, and `size()`.
- Generics catch type mistakes at compile time (`ArrayList<String>` rejects adding anything but a
  `String`).
- Autoboxing/unboxing lets a primitive (e.g., `int`) be used almost transparently with its wrapper
  class (`Integer`) wherever an object type is required, as in `ArrayList<Integer>`.
- Prefer `ArrayList` whenever a collection's size needs to change at run time; prefer a plain
  array when the size is fixed and known.

**You should now be able to:** use `ArrayList` for a growable collection; explain autoboxing and
when to use a wrapper class; choose between an array and an `ArrayList` for a given task.

**This week:** Assignment 2 was assigned (methods, arrays, strings, objects, classes,
`ArrayList`) — see `assignments/assignment-02.md`.

**Next week:** file I/O and exceptions — persisting a program's data between runs.
