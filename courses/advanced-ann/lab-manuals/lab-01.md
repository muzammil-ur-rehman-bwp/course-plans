# Lab Manual 1 — Environment Setup & Open-Problems Landscape Mapping

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Set up the course's PyTorch-first environment, self-assess against the graduate-course
prerequisite checklist, and produce a first-draft research-interest paragraph toward eventually
scoping a capstone problem statement.

## Setup
1. Create and activate a Python 3.10+ virtual environment (`venv` or `conda`).
2. Install `torch`, `numpy`, `matplotlib`, and `jupyter`.
3. Create a working notebook `lab01.ipynb`.

## Procedure
1. **Task A — Environment check:** in `lab01.ipynb`, import `torch` and `numpy`, print their
   versions, and run a trivial GPU-availability check (`torch.cuda.is_available()`), reporting the
   result either way (a CPU-only environment is fine for this course).
2. **Task B — Prerequisite self-assessment:** complete the instructor-provided self-assessment
   checklist against the graduate-course prerequisites (universal approximation, autodiff
   formalism, initialization/normalization theory, optimization-landscape theory, classical
   generalization theory, NTK/Lottery-Ticket/information-bottleneck survey). For any item you are
   not confident recalling, write one sentence noting it for independent review this week.
3. **Task C — Landscape map skim:** read the instructor-provided landscape map of this course's
   12 weekly research topics and their relationship to the graduate course.
4. **Task D — Research-interest paragraph:** write a 150–250 word paragraph naming which 1–2 of
   this course's topics currently interest you most and why. This is a true first draft — it is
   not binding and will evolve well before the Week 8 capstone problem-statement check-in.

## Expected Output
A notebook/document with four clearly labeled sections (A–D), including the completed checklist
and the research-interest paragraph.

## Submission
Submit `lab01.ipynb` (or an accompanying short document for Tasks B–D) via the course submission
system by the end of the lab session.
