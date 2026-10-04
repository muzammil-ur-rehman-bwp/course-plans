# Lab Notes 1 — C++ Basics

**Concept recap:** a C++ program must be compiled (`g++ -std=c++17 -Wall file.cpp -o prog`) and
run as a separate step (`./prog`); `std::cout <<` prints, `std::cin >>` reads; variables must be
declared with a type and should always be initialized.

**Common pitfalls:**
- Forgetting `#include <iostream>` — causes `cout`/`cin` to be "undeclared" compiler errors.
- Forgetting `std::` before `cout`/`cin` (without a `using namespace std;` line, which this
  course avoids to keep namespace usage explicit).
- Declaring a variable without initializing it, then reading its value — undefined behavior, not
  a guaranteed zero.
- Forgetting the executable needs `./` on Linux/macOS to run (`./lab01`, not `lab01`).

**Debugging tip:** read compiler errors from the *first* one reported — later errors are often
just consequences of the first (e.g., a missing semicolon cascades into many unrelated-looking
errors below it).

**Instructor tip:** have students intentionally remove the `#include <iostream>` line once, just
to see and recognize that specific error message — it will recur all semester.
