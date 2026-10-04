# Week 8 Summary — Constraint Satisfaction in Depth; Midterm Review

**Key takeaways:**
- AC-3 enforces arc consistency by revising each arc's domain against its neighbor's domain
  until a fixed point or an empty domain; arc consistency is necessary but not sufficient for a
  solution to exist.
- MRV and the degree heuristic choose which variable to try next during backtracking;
  least-constraining-value chooses which value to try first; none of these change correctness,
  only search efficiency.
- Weeks 1–8 (KR desiderata through CSP) form the midterm's scope.

**You should now be able to:** implement AC-3 from scratch; implement backtracking search with
MRV, degree, and least-constraining-value heuristics; explain the difference between local (arc)
consistency and global solvability.

**Next week:** Midterm Exam, followed by non-monotonic reasoning — the closed-world assumption,
default logic, and circumscription.
