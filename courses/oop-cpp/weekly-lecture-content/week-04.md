# Week 4 — Lecture Content: Operator Overloading II — Stream Operators & `friend`

## 1. Why `operator<<` Can't Be a Member Function
```cpp
std::cout << myFraction;   // what we want to write
```
If `operator<<` were a member of `Fraction`, the *left-hand* operand would have to be the
`Fraction` object (`myFraction.operator<<(std::cout)`), meaning the code would have to be written
`myFraction << std::cout` — backwards. Because the left-hand operand of `<<` here is
`std::ostream` (a type we don't own and can't add members to), `operator<<` must be a **free
function** taking the stream as its first parameter.

## 2. The `friend` Keyword
```cpp
class Fraction {
public:
    Fraction(int num, int den) : num_(num), den_(den) {}

    friend std::ostream& operator<<(std::ostream& out, const Fraction& f);

private:
    int num_, den_;
};
```
Declaring a free function `friend` inside the class grants that *specific* function access to
the class's private members, even though it is not a member function itself. `friend` is a
narrow, deliberate exception to encapsulation — granted to one named function (or class), not to
"everyone outside" — and should be reached for only when a non-member function genuinely needs
private access (as here, where `operator<<` needs `num_`/`den_` but cannot be a member).

## 3. Implementing `operator<<`
```cpp
std::ostream& operator<<(std::ostream& out, const Fraction& f) {
    out << f.num_ << "/" << f.den_;   // private access, granted by `friend`
    return out;                        // MUST return the stream, for chaining
}

std::cout << a << " and " << b << "\n";   // chaining requires operator<< to return std::ostream&
```
Returning `std::ostream&` (the same stream passed in) is what makes `std::cout << a << " and " <<
b` work: each `operator<<` call returns the stream so the next `<<` in the chain has something to
operate on. Forgetting the `return out;` line, or returning by value instead of by reference, is
the most common bug when first writing this operator.

## 4. Implementing `operator>>`
```cpp
std::istream& operator>>(std::istream& in, Fraction& f) {
    int num, den;
    char slash;
    in >> num >> slash >> den;   // expects input like "3/4"
    if (in && slash == '/' && den != 0) {
        f = Fraction(num, den);
    } else {
        in.setstate(std::ios::failbit);   // mark the stream as failed on bad input
    }
    return in;
}
```
`operator>>` takes the target object by **non-`const` reference** (it writes into it) and the
stream by reference (it reads from and advances it). Checking the stream's state (`if (in)`) and
explicitly failing it on malformed input lets calling code detect bad input the same way it would
for `std::cin >> someInt` failing on non-numeric text.

## 5. `friend` vs. Public Accessors: A Design Note
```cpp
// Alternative without friend, using only the public interface:
std::ostream& operator<<(std::ostream& out, const Fraction& f) {
    out << f.getNum() << "/" << f.getDen();   // no private access needed
    return out;
}
```
If a class already exposes adequate public getters, `operator<<` can often be written as an
ordinary free function with no `friend` declaration at all — prefer this when possible. Reach for
`friend` specifically when the operator needs access that would otherwise require adding getters
*only* for the operator's benefit, widening the public interface more than the design otherwise
calls for.

## 6. In-Class Exercise
Write `operator<<` and `operator>>` for the `Complex` class from Week 3 (format: `a+bi`, e.g.
`3+4i`), using `friend`. Test both by printing a `Complex` and by reading one from
`std::cin` with `std::cin >> c`.
