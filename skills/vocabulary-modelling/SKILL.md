---
name: vocabulary-modelling
description: Actively build and sharpen the project's domain model while designing. Challenge terms against VOCABULARY.md, sharpen fuzzy language, stress-test relationships with scenarios, and write terms down as they resolve. Use when a session is changing the model (new terms, renamed concepts, shifting boundaries); reading VOCABULARY.md for definitions needs no skill.
---

Build the project's domain model out loud, with the user, and keep `VOCABULARY.md` current as it takes shape. This is a live discipline, so run it for the whole session, not once at the end.

## The file

`VOCABULARY.md` lives at exactly one path: `<project root>/VOCABULARY.md`. The project root is the git root, or the working directory when there is no repo. Every read and write uses this path, whatever directory the session is in.

It is a glossary and nothing else: no implementation details, no specs, no scratch notes, no decisions. Entry format and rules are in [vocabulary-format.md](vocabulary-format.md). Read it before the first write.

## Create

The first time this skill fires in a session, before crossfire asks its first round, make sure the file exists:

- When it is absent, write `VOCABULARY.md` at the project root from the structure in [vocabulary-format.md](vocabulary-format.md): the context name and description taken from the user's project idea, and an empty `## Language` section.
- When it already exists, Read it and build on it.

Done when the file exists at that path and its contents are in context.

## Challenge

Compare every term the user uses against `VOCABULARY.md`. On a conflict, interrupt whatever round is in progress: say so immediately, ask which meaning holds as the only question, and settle it before any other question is asked. Record the settled term, then hand back to the round.

> Your vocabulary defines "cancellation" as X, but you seem to mean Y. Which is it?

## Sharpen

When a term is vague or overloaded, propose one canonical term and ask the user to confirm it.

> You're saying "account". Do you mean the Customer or the User? Those are different things.

## Stress-test

When relationships between concepts come up, invent a concrete scenario that probes an edge case and forces the user to place the boundary. Prefer the case the user hasn't thought of: a Customer who is also a User, an Order split across two Invoices, a cancellation after delivery. Ask about the scenario itself, with names and numbers, so the answer can't stay abstract.

## Record

The moment a term is finalized, add or update its entry in `VOCABULARY.md`, before asking the next question. One entry per resolution, written as it happens, at every stage of the flow, not only during the first interview. Finalized means the user confirmed the canonical term and its definition. Pick the best word and move the rejected ones under `_Avoid_`.

The session is done when every term the user confirmed is in `VOCABULARY.md` and no conflict raised during the session is still open.
