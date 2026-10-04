# Lab Notes 1 — Java Basics

**Concept recap:** a Java program must be compiled (`javac File.java`, producing `File.class`)
and run as a separate step on the JVM (`java File`); `System.out.println` prints, `Scanner`
reads; variables must be declared with a type and must be initialized before being read.

**Common pitfalls:**
- File name must exactly match the public class name (case-sensitive): `public class Lab01`
  requires the file `Lab01.java`, or it won't compile.
- Forgetting `import java.util.Scanner;` — causes `Scanner` to be "cannot find symbol."
- Running `java Lab01.class` (with the extension) instead of `java Lab01` — the JVM launcher
  takes the class name, not the file name.
- Declaring a variable without initializing it, then reading it — this is a **compile error**
  in Java ("variable might not have been initialized"), not a silent runtime bug; read the error
  and initialize the variable.

**Debugging tip:** read compiler errors from the *first* one reported — later errors are often
just consequences of the first (e.g., a missing semicolon or brace cascades into many unrelated-
looking errors below it).

**Instructor tip:** have students intentionally rename their file so it no longer matches the
public class name, just to see and recognize that specific error — it will recur all semester
when students start multi-file projects.
