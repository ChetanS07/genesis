# Genesis

Genesis is a set of agent skills that takes a project idea to a buildable architecture. It starts with an interview, then produces a system design, a phased tech stack, a low-level design with priced vendor options, and ends by writing the result to files.

Every stage is agreed with you before the next one starts. Nothing moves forward on a guess.

## The flow

```
genesis (orchestrator)
  ├─► crossfire (+ vocabulary-modelling)
  ├─► system-synthesis
  ├─► stack-synthesis
  ├─► ll-design-synthesis
  └─► project-manifest   (on "create")
```

Genesis runs each stage to its approval, takes control back, and starts the next. No stage starts another stage directly.

| Skill | Job | Ends when |
| --- | --- | --- |
| **genesis** | Orchestrator. Takes your idea, runs every stage in order, and asks exit or create at the end | Files are written, or you reply `exit` |
| **crossfire** | Starts vocabulary-modelling, then interviews you in numbered rounds, each question with a recommended answer, until you and the agent share one understanding | You confirm the shared understanding |
| **vocabulary-modelling** | Started by crossfire every time it runs. On its first run it creates `VOCABULARY.md` at the project root, before the first interview round. Challenges conflicting terms, sharpens vague ones, tests edge cases, and writes each term to the file the moment you finalize it, at every stage | Every confirmed term is recorded and no conflict is left open |
| **system-synthesis** | Builds a system design (components, data flow, entities and their relationships, boundaries, key decisions, risks) from the interview answers | You give an explicit yes to the latest design |
| **stack-synthesis** | Picks one product per component, split into at most 3 phases with the MVP as phase 1 | You give an explicit yes to the latest stack |
| **ll-design-synthesis** | Designs each component in detail (interfaces, data, behavior, runtime, observability) and offers three vendors per service need: Starter, Growth, Scale | You approve the latest low-level design |
| **project-manifest** | Writes the approved work into files for a reader who never saw the conversation | All files are written and openable |
| **help-me** | Explains the flow. Only you can start it | Your question is answered |

Type `/genesis` at any point to resume at the first stage without an approved output. You can also type a stage by name; if its input is missing, it hands to genesis to run the earlier stages first.

## Usage

Start with an idea:

```
genesis a booking app for independent yoga studios
```

Without a sentence, genesis asks "What are you building?" first.

- **Answering rounds:** reply to each numbered question. A plain "yes" accepts the recommended answer.
- **Term conflicts:** when a term you use clashes with `VOCABULARY.md`, the round pauses for one question on which meaning holds, then resumes.
- **Approving:** at each agree step, only an explicit yes counts. A yes with a caveat, or "mostly", starts another revision round.
- **Getting help:** type `help-me` at any point.

## Output

The project root is the git root, or the working directory when there is no repo. `VOCABULARY.md` sits directly at the project root and is owned by vocabulary-modelling: created there at the start of the interview, updated through the whole flow, and never attached or sent in any mode. The other three are owned by project-manifest and written to `docs/project-manifest/` on `create`; project-manifest only reads `VOCABULARY.md`:

| File | Contents |
| --- | --- |
| `VOCABULARY.md` | The project glossary. Created when crossfire first starts vocabulary-modelling, then updated each time you finalize a term, at any stage |
| `system-design.md` | The approved system design, including the entity-relationship model: an entity table, a relationship table with cardinality, and a Mermaid ER diagram |
| `tech-stack-and-low-level-design.md` | The phased stack, low-level design (including each component's tables or collections, keys, indexes, and references), vendor picks, and costs |
| `architecture-overview.html` | A single-page overview of the architecture. Shows the full data model: the entity diagram with entity and relationship tables, and each component's tables, keys, indexes, and references. A check script confirms that its entities, relationships, tables, vendors, and totals match the two markdown files |

Any decision you declined to make is written as "Not decided in this session", so the gap stays visible.

## Repository layout

```
skills/
├── genesis/                  # orchestrator
├── crossfire/
├── vocabulary-modelling/     # includes vocabulary-format.md
├── system-synthesis/
├── stack-synthesis/
├── ll-design-synthesis/
├── project-manifest/
└── help-me/
```

Each folder holds one `SKILL.md`. Skills link to each other as sibling folders (`../<name>/SKILL.md`), so keep all eight together.

## Installation

Copy every folder under `skills/` into your agent's skills directory. For Claude Code, that is `~/.claude/skills/` for personal use or `.claude/skills/` inside a project.
