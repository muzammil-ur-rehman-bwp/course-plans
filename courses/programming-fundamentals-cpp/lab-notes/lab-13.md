# Lab Notes 13 — Intro to Classes

**Concept recap:** `class` members default to `private`; a constructor runs automatically when an
object is created and is the right place to validate initial state; member functions access an
object's own private data directly; marking a method `const` promises it won't modify the object.

**Common pitfalls:**
- Making class data `public` "to make it easier" — this defeats encapsulation and lets any code
  bypass the validation the constructor/methods were meant to enforce.
- Forgetting the constructor needs the *same name* as the class and no return type at all (not
  even `void`).
- Writing `withdraw` without checking `amount <= balance`, allowing the balance to go negative —
  exactly the bug encapsulation is meant to prevent, just moved inside the class instead of
  outside it; validation still has to actually happen somewhere.
- Forgetting `const` on a read-only method like `getBalance()`, missing the compiler's help in
  catching an accidental future modification.

**Debugging tip:** if an object's state "isn't updating," check whether the method that should
update it was called on the actual object instance (`account.deposit(50)`) rather than on a copy
or a different array index than intended.

**Instructor tip:** show what happens when `balance` is made `public` and some other function
later sets it directly to a negative number, bypassing `withdraw`'s check entirely — this is the
single most convincing demonstration of why encapsulation matters.
