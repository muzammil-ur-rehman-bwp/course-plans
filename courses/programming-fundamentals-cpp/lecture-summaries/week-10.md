# Week 10 Summary — Structs

**Key takeaways:**
- A `struct` groups related fields into a single user-defined type, avoiding the "parallel arrays
  get out of sync" problem.
- `.` accesses struct members; aggregate initialization (`{val1, val2, ...}`) sets them in
  declaration order.
- An array of structs combines "many items" with "several fields per item" — the pattern behind
  most record-keeping programs, including the capstone.
- Pass structs by (`const`) reference to functions to avoid unnecessary copying.

**You should now be able to:** define a struct; build and process an array of structs; choose
pass-by-value vs. pass-by-reference for struct parameters appropriately.

**Next week:** file I/O — saving and loading a program's data (including arrays of structs) to
and from disk.
