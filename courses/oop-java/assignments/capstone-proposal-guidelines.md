# Capstone Project — Proposal Guidelines

**Due:** Week 11 | **Weight:** 2% of the Capstone grade (20% total course weight)

## What to Submit
A 1–2 page proposal (`capstone_proposal.pdf` or `.md`) covering:
1. **Problem statement**: what domain will your program model (e.g. shapes, inventory items,
   bank accounts), and why does a polymorphic class hierarchy and/or interface fit it naturally?
2. **Class hierarchy design**: the abstract base class and/or interface, and at least two
   concrete classes you plan to implement, naming which methods are abstract/interface methods
   each concrete class must supply.
3. **Generics/Collections component**: which generic type (your own, e.g. a `Box<T>`/`Pair<T,U>`)
   or Collections Framework container (`List`, `Map`) will store or process your objects.
4. **Error handling plan**: at least one realistic error condition your program will detect and
   handle by throwing and catching an exception (standard or custom).
5. **Team**: individual or pair, with each member's planned contribution if a pair.

## Approval
The instructor will respond within one week with approval or requested revisions. Projects must
use techniques covered in this course (inheritance, interfaces/abstract classes, generics/
collections, exception handling) — novel/advanced techniques beyond the syllabus require
instructor pre-approval and are not required for full credit.

## Example Topics (for inspiration, not a closed list)
A shape hierarchy with an area/perimeter calculator (`Shape` abstract class with `Circle`/
`Rectangle`/`Triangle` subclasses), a polymorphic inventory system (an `Item` hierarchy, or an
interface for shared item behavior across physical/digital goods), or a mini banking system with
an account-type hierarchy (`Account` → `CheckingAccount`/`SavingsAccount`, each with different
interest/fee rules) using an interface (e.g. `Transactable`) for transaction behavior — each
driven through superclass- or interface-typed references, each storing its objects in a
`List`/`Map`, and each handling at least one realistic error condition by throwing and catching
an exception.
