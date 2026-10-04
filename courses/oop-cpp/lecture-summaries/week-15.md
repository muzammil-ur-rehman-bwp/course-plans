# Week 15 Summary — RAII & Smart Pointers; Debugging/Testing OOP Code

**Key takeaways:**
- RAII (resources acquired in a constructor, released in a destructor) has been in use since Week
  2; smart pointers apply it specifically to dynamically allocated memory.
- `std::unique_ptr<T>` gives exclusive, move-only ownership — no manual `delete`, and no
  possibility of the Week 2 double-free bug, since it cannot be copied.
- `std::shared_ptr<T>` allows multiple owners via reference counting, freeing the object when the
  last owner is destroyed — use it only when ownership is genuinely shared; default to
  `unique_ptr` otherwise.
- A debugger clarifies constructor/destructor order and smart-pointer lifetime questions; small
  hand-written test functions exercising a class's constructors, operators, and polymorphic
  methods catch bugs (including slicing and missing-`override`) before submission.

**You should now be able to:** replace a raw owning pointer with the appropriate smart pointer,
justify the choice between `unique_ptr` and `shared_ptr`, and write basic test cases for a class.

**Next week:** capstone project presentations and a review of the full course map.
