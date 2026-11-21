---
name: project-manifest
description: Handoff of the approved architecture into files. Fired by genesis when the user replies create, or by the user to write the system design file, the tech stack with low-level design file, an app spec sheet of features and functionality, one HTML overview, and a human-only roadmap from the session context.
---

# Project manifest

Turn the approved system design, tech stack, low-level design, and crossfire Q&A in context into five files: `system-design.md`, `tech-stack-and-low-level-design.md`, `app-spec.md`, `architecture-overview.html`, and `roadmap.md`. Write them for a reader who never saw this conversation and has to build from them.

`roadmap.md` is for humans only. Agents, this skill included, never read it as a source: every other file is built from the context and the two markdown design files, never from the roadmap.

These five files are the whole output. `VOCABULARY.md` belongs to [vocabulary-modelling](../vocabulary-modelling/SKILL.md), which keeps it at `<project root>/VOCABULARY.md`: read it from there for definitions and checks, and leave writing and delivering it to that skill.

Each file has a format reference in `references/` holding its sections, rules, and done criterion:

| File | Format |
| --- | --- |
| `system-design.md` | [references/system-design-format.md](references/system-design-format.md) |
| `tech-stack-and-low-level-design.md` | [references/tech-stack-and-low-level-design-format.md](references/tech-stack-and-low-level-design-format.md) |
| `roadmap.md` | [references/roadmap-format.md](references/roadmap-format.md) |
| `app-spec.md` | [references/app-spec-format.md](references/app-spec-format.md) |
| `architecture-overview.html` | [references/architecture-overview-format.md](references/architecture-overview-format.md) |

## 1. Gather context

Collect from the conversation:

- every crossfire question and answer, gap-fill rounds included
- the approved system design
- the section headed "Approved tech stack", with its phases, picks, assumptions, and risks
- the section headed "Approved low-level design", with its vendor tables, picks, monthly totals, and assumptions

When one is missing, fire [genesis](../genesis/SKILL.md) and end this skill; genesis runs the missing stages and asks before writing files. A design the user has not approved counts as missing.

Done when all four are in context and each is the latest approved version.

Every statement in the files comes from this context.

## 2. Audit gaps

Read the four markdown format references. Every section they name is a slot. Walk the slots one by one and mark each as filled or empty from the context. Treat a slot as empty when the context holds nothing for it, and also when it holds only a vague line such as "standard auth" with no named method.

Group the empty slots into one crossfire round and fire [crossfire](../crossfire/SKILL.md) on them. Then split the answers by effect:

- An answer that only fills a slot (project name, purpose, a missing source URL) goes straight into the files.
- An answer that changes an approved decision returns to the skill that owns it ([system-synthesis](../system-synthesis/SKILL.md) for the design, [stack-synthesis](../stack-synthesis/SKILL.md) for the stack, [ll-design-synthesis](../ll-design-synthesis/SKILL.md) for components and vendors) for re-approval, then this skill resumes.

A slot the user declines to answer, or leaves open after the round, is written as "Not decided in this session", so the reader sees the gap.

Done when no slot is empty without an explicit user decline, and the latest approved versions are in context.

## 3. Pick the destination

- **Claude Code** (the session has no SendUserFile tool): write into `docs/project-manifest/` at the project root. The root is the git root, or the working directory when there is no repo. Create the folder when it is absent.
- **Chat** (SendUserFile is available): write the five files in the working directory and send them in step 10.

When files with these names already exist in the destination, Read them, then replace them. The new run reflects the latest approved design. An existing `roadmap.md` is overwritten from context; its old contents are never a source.

Done when the destination is named.

## 4. Write system-design.md

Read [references/system-design-format.md](references/system-design-format.md) and write the file from the context.

Done when the reference's done criterion holds.

## 5. Write tech-stack-and-low-level-design.md

Read [references/tech-stack-and-low-level-design-format.md](references/tech-stack-and-low-level-design-format.md) and write the file from the context.

Done when the reference's done criterion holds.

## 6. Write roadmap.md

Read [references/roadmap-format.md](references/roadmap-format.md) and write the file from the context.

Done when the reference's done criterion holds.

## 7. Write app-spec.md

Read [references/app-spec-format.md](references/app-spec-format.md) and write the file from the context and the two markdown design files; the roadmap is not a source.

Done when the reference's done criterion holds.

## 8. Build architecture-overview.html

Load frontend-design with the Skill tool when it is listed. Read [references/architecture-overview-format.md](references/architecture-overview-format.md) and build from `system-design.md` and `tech-stack-and-low-level-design.md` so the three stay in agreement. The roadmap is not an input.

Done when the reference's done criterion holds.

## 9. Check

Run a script that:

- finds every monthly total and vendor name from `tech-stack-and-low-level-design.md` in the HTML
- re-adds each phase total from that phase's per-need figures
- finds every component name from `system-design.md` in the other two files
- finds every entity name from `system-design.md` in `tech-stack-and-low-level-design.md` and the HTML
- finds every relationship from `system-design.md` in the HTML with the same two entities and the same cardinality
- finds every table or collection name from `tech-stack-and-low-level-design.md` in the HTML, under the component that owns it
- finds every phase and component name from `tech-stack-and-low-level-design.md` in `roadmap.md`, under the same phase, and confirms its first lines are the human-only note
- finds every component, entity, and phase name from `system-design.md` and `tech-stack-and-low-level-design.md` in `app-spec.md`
- confirms that `app-spec.md` follows the section order of the template in `references/app-spec-format.md`, that its story numbers run without gaps, that every story number in Delivery Phases exists, that every story is in exactly one phase or in Out of Scope, and that the Status line is `ready-for-agent` only when Open Decisions is empty
- finds every entity name from `system-design.md` as an entry in `VOCABULARY.md` (one direction only: vocabulary terms that are not entities are expected and pass)

Fix each mismatch at its source, rebuild what depends on it, and run the script again. When an entity has no `VOCABULARY.md` entry, fire [vocabulary-modelling](../vocabulary-modelling/SKILL.md) to settle and record the term with the user; when the settled name differs from the design's, carry it back into the five files.

Done when the script reports zero mismatches.

## 10. Deliver

- **Claude Code:** state the folder path and the five file paths, noting that `roadmap.md` is for humans only.
- **Chat:** call SendUserFile with all five files, the markdown files with `display: "attach"` and the HTML with `display: "render"`.

Close with two sentences: what was written and where, and how many "Not decided in this session" gaps the files hold.

Done when the user can open all five files.
