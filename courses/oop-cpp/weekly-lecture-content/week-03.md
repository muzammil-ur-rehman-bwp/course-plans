# Week 3 — Lecture Content: Operator Overloading I

## 1. Why Overload Operators?
```cpp
Fraction sum = a.add(b);   // works, but reads unlike ordinary arithmetic
Fraction sum = a + b;      // what we want — same meaning, natural syntax
```
C++ lets user-defined types participate in the same operator syntax built-in types use, as long
as the class defines what the operator means for it. This is not "magic" — `a + b` for a class
type is just a call to a function named `operator+`, written by you.

## 2. Arithmetic Operators as Member Functions
```cpp
class Fraction {
public:
    Fraction(int num, int den) : num_(num), den_(den) {}

    Fraction operator+(const Fraction& rhs) const {
        return Fraction(num_ * rhs.den_ + rhs.num_ * den_, den_ * rhs.den_);
    }

    Fraction operator-(const Fraction& rhs) const {
        return Fraction(num_ * rhs.den_ - rhs.num_ * den_, den_ * rhs.den_);
    }

    int getNum() const { return num_; }
    int getDen() const { return den_; }

private:
    int num_, den_;
};

Fraction a(1, 2), b(1, 3);
Fraction c = a + b;   // calls a.operator+(b); c is a NEW Fraction, a and b are unchanged
```
As a member function, `operator+` is declared `const` (it does not modify `*this`) and returns a
**new** `Fraction` by value — it must not mutate `a` or `b`. The left-hand operand (`a`) is the
implicit object the method is called on; the right-hand operand (`b`) is the parameter.

## 3. Compound Assignment, Then Arithmetic in Terms of It
```cpp
class Fraction {
public:
    Fraction& operator+=(const Fraction& rhs) {
        num_ = num_ * rhs.den_ + rhs.num_ * den_;
        den_ = den_ * rhs.den_;
        return *this;              // enables chaining: a += b += c
    }

    Fraction operator+(const Fraction& rhs) const {
        Fraction result(*this);    // copy *this (Week 2's copy constructor)
        result += rhs;             // reuse operator+=
        return result;
    }
    // ...
};
```
`operator+=` *does* mutate `*this` and returns a reference to it (`Fraction&`, not a new object)
so compound-assignment chains work. A common, DRY pattern is to implement `operator+=` first and
then implement `operator+` by copying and delegating to it, as shown — one place owns the actual
arithmetic logic.

## 4. Comparison Operators
```cpp
class Fraction {
public:
    bool operator==(const Fraction& rhs) const {
        return num_ * rhs.den_ == rhs.num_ * den_;   // cross-multiply; avoids needing reduced form
    }

    bool operator<(const Fraction& rhs) const {
        return num_ * rhs.den_ < rhs.num_ * den_;    // assumes positive denominators
    }
    // ...
};

Fraction a(1, 2), b(2, 4);
if (a == b) { /* true: 1/2 and 2/4 are equal fractions */ }
```
`operator==` and `operator<` should be defined **consistently**: if `a == b` is true, `a < b`
must be false, and vice versa. Getting this wrong silently breaks any code that sorts or searches
a collection of your type (algorithms assume this consistency and will not warn you).

## 5. A Complete Mini Example: `Complex`
```cpp
class Complex {
public:
    Complex(double re, double im) : re_(re), im_(im) {}

    Complex operator+(const Complex& rhs) const {
        return Complex(re_ + rhs.re_, im_ + rhs.im_);
    }

    Complex operator*(const Complex& rhs) const {
        return Complex(re_ * rhs.re_ - im_ * rhs.im_,
                        re_ * rhs.im_ + im_ * rhs.re_);
    }

    bool operator==(const Complex& rhs) const {
        return re_ == rhs.re_ && im_ == rhs.im_;
    }

    double real() const { return re_; }
    double imag() const { return im_; }

private:
    double re_, im_;
};
```
Note that `operator*` here implements true complex multiplication, not element-wise
multiplication — overloading an operator should match the mathematical or domain meaning readers
will expect; overloading `+` to mean something unrelated to addition is a well-known anti-pattern.

## 6. In-Class Exercise
Add `operator-` and `operator!=` to the `Complex` class above (`operator!=` can simply be `return
!(*this == rhs);`), then write a small `main` that builds two `Complex` values and prints the
result of each operator.
