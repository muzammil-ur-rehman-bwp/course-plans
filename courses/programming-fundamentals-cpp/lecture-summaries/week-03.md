# Week 3 Summary — Control Flow: `if`/`else`, `switch`

**Key takeaways:**
- `if`/`else if`/`else` chains are checked top to bottom; order the conditions carefully.
- Always brace conditional bodies, even single statements, to avoid the dangling-`else`/silent-
  scope bug.
- `switch` tests equality against constants only; forgetting `break` causes fall-through, which is
  sometimes intentional (grouped cases) but often a bug.
- The ternary operator `cond ? a : b` is a concise alternative for simple either/or expressions.

**You should now be able to:** translate a decision table into correct `if`/`else` or `switch`
code; explain `switch` fall-through and when to use `break`.

**Next week:** loops (`for`, `while`, `do-while`) — repeating logic instead of just branching once.
