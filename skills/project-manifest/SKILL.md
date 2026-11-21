---
name: project-manifest
description: Handoff of the approved architecture into files. Fired by genesis when the user replies create, or by the user to write the system design file, the tech stack with low-level design file, and one HTML overview from the session context.
---

# Project manifest

Turn the approved system design, tech stack, low-level design, and crossfire Q&A in context into three files: `system-design.md`, `tech-stack-and-low-level-design.md`, and `architecture-overview.html`. Write them for a reader who never saw this conversation and has to build from them.

These three files are the whole output. `VOCABULARY.md` belongs to [vocabulary-modelling](../vocabulary-modelling/SKILL.md), which keeps it at `<project root>/VOCABULARY.md`: read it from there for definitions and checks, and leave writing and delivering it to that skill.

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

Every section named in steps 4 and 5 is a slot. Walk the slots one by one and mark each as filled or empty from the context. Treat a slot as empty when the context holds nothing for it, and also when it holds only a vague line such as "standard auth" with no named method.

Group the empty slots into one crossfire round and fire [crossfire](../crossfire/SKILL.md) on them. Then split the answers by effect:

- An answer that only fills a slot (project name, purpose, a missing source URL) goes straight into the files.
- An answer that changes an approved decision returns to the skill that owns it ([system-synthesis](../system-synthesis/SKILL.md) for the design, [stack-synthesis](../stack-synthesis/SKILL.md) for the stack, [ll-design-synthesis](../ll-design-synthesis/SKILL.md) for components and vendors) for re-approval, then this skill resumes.

A slot the user declines to answer, or leaves open after the round, is written as "Not decided in this session", so the reader sees the gap.

Done when no slot is empty without an explicit user decline, and the latest approved versions are in context.

## 3. Pick the destination

- **Claude Code** (the session has no SendUserFile tool): write into `docs/project-manifest/` at the project root. The root is the git root, or the working directory when there is no repo. Create the folder when it is absent.
- **Chat** (SendUserFile is available): write the three files in the working directory and send them in step 8.

When files with these names already exist in the destination, Read them, then replace them. The new run reflects the latest approved design.

Done when the destination is named.

## 4. Write system-design.md

Sections, in order:

1. Project: name, purpose, users, taken from the crossfire answers.
2. Requirements: functional, non-functional, and constraints (budget, expected users, compliance, timeline).
3. Architecture: each component with its responsibility, owned data, and dependencies, plus a Mermaid diagram of components and flows.
4. Entities and relationships: a table of every entity with its owning component, key attributes, and a one-line definition taken from `VOCABULARY.md`. Then a table of every relationship with both entities, its cardinality, and whether it crosses a component boundary. Then a Mermaid `erDiagram` of the entities and relationships.
5. Journeys: each main journey step by step through the components, failure paths included.
6. Integrations: every external system and what crosses the boundary.
7. Decisions: each with its reason and the crossfire answer behind it.
8. Assumptions and open items.
9. Appendix: the crossfire Q&A in full, in the order asked.

Done when every component, entity, relationship, journey, requirement, constraint, integration, and decision in the approved design has a place, and every crossfire question appears with its answer.

## 5. Write tech-stack-and-low-level-design.md

Sections, in order:

1. Phases: each phase with its components, expected users, and monthly budget, MVP first.
2. Tech stack: one row per technology pick, grouped by phase, with the layer (frontend, backend, data, infrastructure, tooling, or any other layer decided), the technology, its hosting model, its version when settled, the reason, the alternative considered, and the lock-in it brings.
3. Low-level design: first the service marks, each with its capability, phase, and why buying beats building; then one entry per component, named exactly as in `system-design.md`, holding its phase, whether it is built or bought, interfaces, data, behavior, runtime, and observability as approved.
4. Vendors and pricing: for each service need, the stack pick or service mark it serves and a table with Starter, Growth, and Scale options. Each option shows the vendor and plan, monthly cost at each phase's expected usage, the limit that forces the next tier, the cost of leaving, the source URL, and the date of the figure. Below the table, the pick for each phase with its reason and the crossfire answer behind it.
5. Monthly totals: each phase's picked mix against that phase's budget, and each tier taken whole, with the arithmetic shown.
6. Assumptions and risks, from both the stack and the low-level design.

Copy figures unchanged from the approved low-level design, and keep the date each was gathered, so the reader can judge how stale it is.

Done when every phase, pick, service mark, component, and service need is present, and every price carries its source and date.

## 6. Build architecture-overview.html

Load frontend-design with the Skill tool when it is listed. Build from the two markdown files so the three stay in agreement.

Four sections, in this order, behind a sticky section nav:

1. **About**: one paragraph of three to five sentences on what the project is, who it serves, and what it does.
2. **System design**: an inline SVG of the components and their flows, a component table, an inline SVG of the entities and relationships with their cardinality, an entity table (owning component, key attributes, one-line definition) and a relationship table (both entities, cardinality, whether it crosses a component boundary) copied from `system-design.md`, and a short line per journey.
3. **Tech stack**: the phases with their components, then a table of technology picks grouped by phase with layer, technology, hosting model, and reason.
4. **Low-level design and pricing**: the service marks with capability and phase, then one collapsible entry per component, each showing whether it is built or bought, its interfaces and its data: every table or collection it owns with keys, indexes, and constraints, and every reference to another entity (foreign key, join table, or embedded reference, and for cross-component references how consistency is kept), copied from `tech-stack-and-low-level-design.md`; then per service need a tier comparison with the picked option marked, then monthly totals for each phase's picked mix and for each tier.

One self-contained file: inline CSS and JS, no network resources, readable at phone width, light and dark through `prefers-color-scheme`, and clean when printed.

Done when the four sections appear in order and every phase, component name, entity, relationship, table, vendor, price, and total matches the markdown text exactly.

## 7. Check

Run a script that:

- finds every monthly total and vendor name from `tech-stack-and-low-level-design.md` in the HTML
- re-adds each phase total from that phase's per-need figures
- finds every component name from `system-design.md` in the other two files
- finds every entity name from `system-design.md` in `tech-stack-and-low-level-design.md` and the HTML
- finds every relationship from `system-design.md` in the HTML with the same two entities and the same cardinality
- finds every table or collection name from `tech-stack-and-low-level-design.md` in the HTML, under the component that owns it
- finds every entity name from `system-design.md` as an entry in `VOCABULARY.md` (one direction only: vocabulary terms that are not entities are expected and pass)

Fix each mismatch at its source, rebuild what depends on it, and run the script again. When an entity has no `VOCABULARY.md` entry, fire [vocabulary-modelling](../vocabulary-modelling/SKILL.md) to settle and record the term with the user; when the settled name differs from the design's, carry it back into the three files.

Done when the script reports zero mismatches.

## 8. Deliver

- **Claude Code:** state the folder path and the three file paths.
- **Chat:** call SendUserFile with all three files, the markdown files with `display: "attach"` and the HTML with `display: "render"`.

Close with two sentences: what was written and where, and how many "Not decided in this session" gaps the files hold.

Done when the user can open all three files.
