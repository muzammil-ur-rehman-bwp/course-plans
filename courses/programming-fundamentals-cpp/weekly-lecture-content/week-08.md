# Week 8 — Lecture Content: Pointers & References; Midterm Review

## 1. Addresses and the `&` Operator
Every variable lives at a memory address. The address-of operator `&` retrieves it:
```cpp
int x = 5;
std::cout << &x << "\n";  // prints an address, e.g., 0x7ffeec...
```

## 2. Pointers
A pointer is a variable that **stores an address**:
```cpp
int x = 5;
int* p = &x;          // p holds the address of x
std::cout << *p << "\n";   // 10? no — *p dereferences p, giving x's value: 5
*p = 10;                // writes through the pointer: x is now 10
std::cout << x << "\n";    // 10

int* q = nullptr;      // a pointer that points to nothing — always initialize pointers
```
`*` means two different things depending on context: in a declaration (`int* p`) it marks `p` as a
pointer type; in an expression (`*p`) it dereferences the pointer, accessing the value it points
to. A pointer that is never assigned a valid address should be set to `nullptr`, and must be
checked before being dereferenced.

## 3. Pointers and Arrays
An array's name, used in most expressions, decays to a pointer to its first element:
```cpp
int values[4] = {10, 20, 30, 40};
int* p = values;             // p now points to values[0]
std::cout << *p << "\n";     // 10
std::cout << *(p + 1) << "\n";  // 20 — pointer arithmetic moves by element size, not by byte
std::cout << p[2] << "\n";       // 30 — p[i] works the same as values[i]
```

## 4. Pointers vs. References
```cpp
int x = 5;
int& ref = x;    // reference: an alias for x, must be bound at declaration, cannot be null
int* ptr = &x;   // pointer: a separate variable holding x's address, can be reseated or null
```
| | Reference (`&`) | Pointer (`*`) |
|---|---|---|
| Must be initialized | Yes, at declaration | No (but should be, to `nullptr` if unused) |
| Can be null | No | Yes (`nullptr`) |
| Can be reseated (point elsewhere later) | No | Yes |
| Syntax at use site | Same as the variable (`ref`) | Needs `*` to dereference (`*ptr`) |

References (introduced in Week 5 as `&` parameters) are generally preferred when a function always
needs a valid value to work with; pointers are used when "no value" (`nullptr`) is a meaningful
possibility, or when the thing pointed to needs to change over time.

## 5. A Pointer Parameter Example
```cpp
void increment(int* n) {
    if (n != nullptr) {
        (*n)++;
    }
}

int main() {
    int x = 5;
    increment(&x);
    std::cout << x << "\n";  // 6
}
```
Compare this to the reference version from Week 5 (`void increment(int& n)`); the reference
version is simpler to call (`increment(x)`, no `&`) and cannot be null, which is why this course
prefers references for "must always be valid" parameters and reserves raw pointers for cases
where null is meaningful (as with dynamic memory, next week).

## 6. Midterm Review: Topics Covered (Weeks 1–7)
- Compiling, variables, primitive types (Week 1)
- Operators, expressions, type conversion (Week 2)
- `if`/`else`, `switch` (Week 3)
- `for`, `while`, `do-while` (Week 4)
- Functions: value vs. reference parameters, overloading (Week 5)
- 1D arrays (Week 6)
- 2D arrays, C-strings vs. `std::string` (Week 7)

## 7. In-Class Exercise
Write a function `void findMinMax(const int values[], int size, int* minOut, int* maxOut)` that
writes the minimum and maximum of the array through the two output pointer parameters, then work
through 3–4 midterm-style practice problems as a class.
