---
name: genesis
description: Orchestrator of the genesis protocol, from project idea to written architecture files. Use when the user wants to design, architect, or plan a new project or product from scratch, names a project idea and asks where to begin, or wants to continue the flow from where it stopped.
---

# Genesis

The orchestrator. Take the user's project idea and run every stage in order, each to its own approval, until the files are written or the user exits. Genesis is the only skill that moves the flow forward: a stage hands control back here when it ends, and genesis starts the next one.

## Stages

| # | Stage | Output it leaves in context | Ends when |
| --- | --- | --- | --- |
| 1 | [crossfire](../crossfire/SKILL.md) | the crossfire Q&A, and `<project root>/docs/project-manifest/vocabulary.md`, created by vocabulary-modelling before the first round and updated each time a term is finalized | the user confirms a shared understanding |
| 2 | [system-synthesis](../system-synthesis/SKILL.md) | the approved system design | an explicit yes to the latest design |
| 3 | [stack-synthesis](../stack-synthesis/SKILL.md) | the section headed "Approved tech stack" | an explicit yes, restated under that heading |
| 4 | [ll-design-synthesis](../ll-design-synthesis/SKILL.md) | the section headed "Approved low-level design" | an explicit yes, restated under that heading |
| 5 | [project-manifest](../project-manifest/SKILL.md) | the written files | the user can open every file |

## 1. Take the idea

Read the conversation for the project. Carry the user's words forward unedited.

When neither the message nor the conversation names a project, ask one question, "What are you building?", and wait for the answer.

Done when the project idea sits in context in the user's own words.

## 2. Find the starting stage

Walk stages 1 to 4 in order and check each output against the table. An output that exists but was never approved counts as missing. The first stage with a missing output is the starting stage. When stages 1 to 4 are all complete, go to step 4.

Done when the starting stage is named.

## 3. Run the stages

From the starting stage through stage 4, for each stage in order:

1. Say in one line which stage starts and what it produces.
2. Fire it, passing the idea and every earlier output in full.
3. Wait until it ends on its own completion condition from the table.

A stage may fire an earlier stage to re-approve a decision it reopened; that stage returns to the one that fired it, and genesis carries on once the current stage ends.

Done when stage 4 has ended with "Approved low-level design" restated in context.

## 4. Exit or create

Ask exactly: "Exit, or create the artifacts (system design file, tech stack with low-level design file, one HTML file combining both)? Reply with one word: exit or create."

Any reply other than a clear "exit" or "create", such as a partial selection or "maybe", gets the same question again.

- **create:** fire [project-manifest](../project-manifest/SKILL.md), passing everything in context in full: every crossfire question and answer from the first run onward, the gap-fill rounds, the approved system design, the approved tech stack, and the approved low-level design.
- **exit:** end with all of that left in the conversation. Tell the user that typing `/genesis` later resumes at file writing.

Done when the user has replied "exit" or "create" and the matching branch has run to its end.
