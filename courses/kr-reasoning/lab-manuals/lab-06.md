# Lab Manual 6 — Semantic Networks and Frames with Inheritance

**Duration:** 3 hours | **Prerequisite:** Week 6 lecture

## Objectives
Implement a semantic network with property inheritance and a frame system with slots, defaults,
and correct default-overriding inheritance.

## Setup
Create `lab06.ipynb`.

## Procedure
1. **Task A — Semantic network:** implement `SemanticNetwork` (`add_isa`, `set_property`,
   `get_property`) from the lecture content; build a 4-level IS-A hierarchy of your choosing and
   demonstrate strict inheritance both working correctly (a property with no exception) and
   failing (reproduce a Tweety/penguin-style wrong answer).
2. **Task B — Frame class:** implement `Frame` (`set_slot`, `set_default`, `get_slot`) from the
   lecture content.
3. **Task C — Fixing the exception:** rebuild the same hierarchy from Task A as a frame hierarchy,
   using defaults at the general level and explicit slot overrides at the exception level; show
   `get_slot` now resolves correctly where the semantic network in Task A did not.
4. **Task D — Three-level override:** add a third level below your exception (e.g.,
   `EmperorPenguin` below `Penguin`) that sets neither the slot nor the default itself; trace,
   in a markdown cell, which ancestor frame's value it resolves to and why.

## Expected Output
A notebook with Tasks A–D; a working semantic network demonstrating the exceptions problem, and a
working frame system that correctly resolves the same case via default-overriding inheritance.

## Submission
Submit `lab06.ipynb` by the end of the lab session.
