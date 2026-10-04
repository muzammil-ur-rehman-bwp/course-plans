# Week 14 Lecture Plan — Introduction to AI
## Topic: Natural Language Processing Overview

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why natural language is hard for AI (ambiguity, context, world knowledge). (*Understand*)
2. Apply tokenization and the bag-of-words representation to convert text into a form a program can process. (*Apply*)
3. Evaluate, at a survey level, where large language models fit in the broader AI landscape covered this semester. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:25 | Why language is hard | Lexical, syntactic, and semantic ambiguity; the role of context and world knowledge |
| 0:25–0:50 | Tokenization | Splitting text into tokens; punctuation/casing/stop-word considerations (brief) |
| 0:50–1:00 | Break | — |
| 1:00–1:30 | Bag-of-words | Representing a document as a word-count vector; limits (word order is lost) |
| 1:30–2:00 | NLP in the AI landscape | From bag-of-words to n-grams to today's large language models (survey level, no implementation); connecting back to search/logic/learning themes from the course |

### Materials/Equipment
- Slides: ambiguous-sentence examples, bag-of-words worked example
- Starter notebook: tokenizer and bag-of-words vectorizer skeleton

### Formative Check (in-class)
Exercise: tokenize two short sentences by hand, build their bag-of-words vectors, and discuss
one piece of information the representation loses (e.g., negation, word order).

### Link to Lab/Assessment
Lab 14: Implement a simple tokenizer and bag-of-words vectorizer from scratch; use it for a toy
text-classification demo.
