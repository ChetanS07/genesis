---
name: help-me
description: Explains how the genesis plugin works, which skills run in what order, and what you do at each step.
disable-model-invocation: true
---

# Help me

Answer the user's question about the genesis plugin from the reference below. Lead with the answer. When the question is open ("how does this work?"), walk the flow in order.

Done when the user's question is answered and they know what to do next.

## The flow

Genesis is the orchestrator. It runs crossfire (with vocabulary-modelling alongside), then system-synthesis, then stack-synthesis, then ll-design-synthesis, then project-manifest, each to its own approval. Each stage hands control back to genesis when it ends, and genesis starts the next one.

Every skill except help-me can be fired by the agent, and you can type any of them as a slash command (`/system-synthesis`) to run that stage. When its input is missing from the conversation, it fires genesis, which runs the missing stages first. Typing `/genesis` mid-flow resumes at the first stage without an approved output.

| Skill | Started by | Job | Ends when |
| --- | --- | --- | --- |
| genesis | you or the agent | Takes your project idea and runs every stage below in order | Files are written, or you reply "exit" |
| crossfire | genesis, any later stage with a gap, or the agent | Starts vocabulary-modelling, then interviews you in rounds until you and the agent share one understanding | You confirm the understanding |
| vocabulary-modelling | crossfire, every time it starts | Creates `docs/project-manifest/vocabulary.md` at the project root on its first run, then writes each term you finalize to it | Every confirmed term is recorded and no conflict is open |
| system-synthesis | genesis, or you | Builds a system design from the crossfire answers | You say an explicit yes to the latest design |
| stack-synthesis | genesis, or you | Picks one product per component, split into at most 3 phases with the MVP first | You say an explicit yes to the latest stack |
| ll-design-synthesis | genesis, or you | Designs each component in detail and offers three vendors per service need (Starter, Growth, Scale) with cited, dated prices | You approve the latest low-level design |
| project-manifest | genesis on "create", or you | Writes the approved work into three files | You can open all three |
| help-me | you only | This reference | Your question is answered |

## Starting

Type `/genesis` followed by a sentence about what you are building. Without the sentence, it asks "What are you building?" first.

Genesis fires crossfire first, and crossfire starts vocabulary-modelling. Before the first round, vocabulary-modelling creates `docs/project-manifest/vocabulary.md` at the project root (the git root, or the working directory when there is no repo); when the file already exists, it builds on it. Crossfire then interviews you in rounds. Vocabulary-modelling runs alongside it for the rest of the flow, flags terms that conflict with `vocabulary.md` or sound vague, and writes each term to the file the moment you finalize it. When a term you use conflicts with `vocabulary.md`, it interrupts the round and asks which meaning holds as a single question; once you settle it, the round resumes. Its other questions (sharpening a vague term, edge-case scenarios) arrive inside crossfire's rounds, so you answer one numbered set at a time. A "yes" accepts the recommended answer under a question. The interview ends when you confirm that you and the agent share one understanding.

## Moving to the system design

After you confirm in crossfire, genesis says in one line that system-synthesis is next and starts it. It builds the system design from your crossfire answers and asks "Is any change needed, or does it serve your project well?" until you approve.

The same approval rule holds at every agree step in the flow: a yes with a caveat, or "mostly", counts as a change request and starts another round.

## After the low-level design

Once you approve the low-level design and its vendor picks, the agent restates the approved version under the heading "Approved low-level design", with the component entries, vendor picks, monthly totals, and assumptions. Genesis then asks you to reply with one word:

- **create**: it fires project-manifest, which writes the three files from everything in the conversation.
- **exit**: the flow ends and everything stays in the conversation. Type `/genesis` later to get the same question again.

Any other reply, such as "maybe", gets the same question again.

## What you get

Two skills write files, each owning its own:

- **vocabulary-modelling** owns `vocabulary.md`: always at `docs/project-manifest/vocabulary.md` in the project root, created at the start and kept current through every stage. It is never sent as an attachment; it stays at that path in every mode.
- **project-manifest** owns three files, written on "create": `system-design.md`, `tech-stack-and-low-level-design.md`, and `architecture-overview.html`. In Claude Code they land in `docs/project-manifest/` at the project root. In chat the two markdown files arrive as attachments and the HTML is rendered. The HTML shows the full data model: the entity diagram with entity and relationship tables (owner, key attributes, cardinality), and under each component its tables or collections with keys, indexes, and references. Before delivery, a check script confirms that every entity, relationship, table, vendor, and total in the HTML matches the markdown files. It reads `vocabulary.md` for entity definitions and checks, but never creates, changes, or delivers it.

Any detail the session never settled is written as "Not decided in this session", so a reader sees the gap.

## Where the skills live

Each skill is its own folder with one `SKILL.md`, side by side:

```
skills/
├── genesis/
├── crossfire/
├── vocabulary-modelling/
├── system-synthesis/
├── stack-synthesis/
├── ll-design-synthesis/
├── project-manifest/
└── help-me/
```

Install all eight together in your skills directory (`~/.claude/skills/` or a project's `.claude/skills/` in Claude Code). Skills hand off to each other through `../<name>/SKILL.md` links, so a missing folder breaks the flow at that step.

## Good to know

- The design lives in the conversation; only `vocabulary.md` is on disk from the start. A new conversation starts without the design, so write the files before closing one you want to keep.
- A stage started on its own looks in the conversation for the earlier outputs it needs. When one is missing, it hands to genesis, which runs the missing stages. When the gap is a single fact, the stage fires crossfire on it instead.
- A change late in the flow can reopen an earlier decision. The agent sends it back to the stage that owns it for re-approval; that stage returns to the one that sent it, which then resumes.
