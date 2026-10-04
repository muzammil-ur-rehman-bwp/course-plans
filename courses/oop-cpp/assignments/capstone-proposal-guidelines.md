# Capstone Project — Proposal Guidelines

**Due:** Week 11 | **Weight:** 2% of the Capstone grade (20% total course weight)

## What to Submit
A 1–2 page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Problem statement**: what domain will your program model (e.g. shapes, inventory items,
   bank accounts), and why does a polymorphic class hierarchy fit it naturally?
2. **Class hierarchy design**: the abstract base class and at least two concrete derived classes
   you plan to implement, with the key virtual (and at least one pure virtual) methods each will
   provide.
3. **Generic/STL component**: which template (your own, e.g. a `Stack<T>`/`Pair<T,U>`) or STL
   container (`std::vector`, `std::map`) will store or process your objects.
4. **Error handling plan**: at least one realistic error condition your program will detect and
   handle by throwing and catching an exception (standard or custom).
5. **Team**: individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
use techniques covered in this course (inheritance, polymorphism, templates/STL, exception
handling, and ideally smart pointers) — novel/advanced techniques beyond the syllabus require
instructor pre-approval and are not required for full credit.

## Example Topics (for inspiration, not a closed list)
A shape hierarchy with an area/perimeter calculator (`Shape` → `Circle`/`Rectangle`/`Triangle`),
a small inventory system with polymorphic item types (e.g. `Item` → `PhysicalGood`/`DigitalGood`,
each with different shipping/delivery behavior), or a mini banking system with an account-type
hierarchy (`Account` → `CheckingAccount`/`SavingsAccount`, each with different interest/fee
rules) — each driven through base-class pointers/references, each storing its objects in an STL
container, and each handling at least one realistic error condition by throwing and catching an
exception.
