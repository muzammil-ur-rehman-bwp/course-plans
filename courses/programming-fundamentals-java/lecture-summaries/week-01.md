# Week 1 Summary — Intro to Programming & Java Basics

**Key takeaways:**
- A Java program must be compiled to bytecode (`javac`) and run on the JVM (`java`) — the same
  `.class` file can run on any machine with a compatible JVM.
- Every Java program lives inside a `class`, with `public static void main(String[] args)` as
  its entry point; `System.out.println` prints, `Scanner` reads input.
- Java is statically typed: `int`, `double`, `char`, and `boolean` are core primitive types, each
  with a fixed size and meaning.
- The compiler enforces "definite assignment" — reading a local variable before it is initialized
  is a compile error, not a runtime surprise.

**You should now be able to:** compile and run a Java program from the command line; declare and
initialize variables of primitive types; read values with `Scanner` and print results with
`System.out`.

**Next week:** operators, expressions, and type conversion — how Java combines variables into
computations, and the primitive-vs-reference-type distinction.
