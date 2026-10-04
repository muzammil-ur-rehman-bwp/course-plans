# Lab Manual 15 — A Knowledge-Graph Triple Store with Pattern Queries

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Implement a knowledge-graph triple store with multi-hop pattern queries; compare it to the Week 6
semantic network.

## Setup
Create `lab15.ipynb`.

## Procedure
1. **Task A — KnowledgeGraph class:** implement `KnowledgeGraph` (`add`, `query`, `path_query`)
   from the lecture content.
2. **Task B — Build a graph:** populate a knowledge graph of at least 15 triples over a domain of
   your choosing (e.g., people, organizations, locations), with at least 3 distinct predicates
   usable in a multi-hop chain.
3. **Task C — Pattern queries:** write 3 `path_query` calls of length 2 or more (e.g.,
   `["worksFor", "locatedIn"]`) and report their results.
4. **Task D — Comparison:** rebuild a small portion of your graph (at least 5 facts) as a Week 6
   `SemanticNetwork`/`Frame` hierarchy instead, and write 3–4 sentences comparing the two
   representations for this data — which is more natural, and which supports the multi-hop query
   from Task C more directly.

## Expected Output
A notebook with Tasks A–D; a working knowledge graph with 3 correct multi-hop queries, and a
short, concrete comparison against a semantic-network/frame rebuild of part of the same data.

## Submission
Submit `lab15.ipynb` by the end of the lab session.
