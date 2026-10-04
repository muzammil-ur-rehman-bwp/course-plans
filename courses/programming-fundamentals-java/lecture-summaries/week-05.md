# Week 5 Summary — Methods

**Key takeaways:**
- A method's declaration has a return type, name, and parameter list; decomposing a program into
  small methods makes it easier to read, test, and debug.
- Java is always pass-by-value — but for a reference-type parameter (an array or object), the
  *reference* is what gets copied, so changes to the referenced object's contents are visible to
  the caller, while reassigning the parameter itself is not.
- Method overloading lets several methods share a name as long as their parameter lists differ;
  resolution happens at compile time based on the arguments.
- Local variables (including parameters) exist only for the duration of their method call.

**You should now be able to:** write and call methods with parameters and return values; predict
whether a method call can change a caller's primitive variable vs. an object's contents; write
overloaded methods.

**Next week:** arrays — storing and processing collections of values.
