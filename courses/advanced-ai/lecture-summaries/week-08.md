# Week 8 Summary — AI Safety and Alignment I

**Key takeaways:**
- Specification gaming/reward hacking is a technical Goodhart's-law phenomenon: optimization
  pressure finds and exploits gaps between a specified proxy reward and the true intended goal
  (e.g., the well-documented CoastRunners case study).
- The alignment problem asks whether a system's behavior matches the designer's true intent, not
  merely its specified training objective.
- Outer alignment (is the specified objective a good proxy?) and inner alignment (did the system
  learn the specified objective, or a different internal mesa-objective?) are distinct failure
  modes; inner-alignment failures can be invisible throughout training.

**You should now be able to:** classify a failure as outer alignment, inner alignment, or both;
construct a toy specification-gaming demonstration.

**Next week:** Midterm Exam, then AI safety and alignment II — reward modeling, scalable
oversight, and interpretability as a safety tool.
