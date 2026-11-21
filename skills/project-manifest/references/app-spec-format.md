# app-spec.md format

The spec sheet for the app: every feature and how it behaves, written so an agent can build from it without asking. Follow the template's sections, order, and headings exactly.

## Template

```markdown
Status: draft | ready-for-agent
# <App> v1: <one-line description>

Terms: VOCABULARY.md · Decisions: docs/adr/ · Last updated: <date>

## Problem Statement
Who has the problem, what happens today, why that hurts. 1 paragraph.

## Solution
What you're building, in plain words. 1–2 paragraphs. No tech.

## User Stories
Grouped by actor, numbered across the whole doc.
1. As a <actor>, I want <capability>, so that <outcome>.

## Implementation Decisions
### Architecture        apps/services, hosting, environments, why
### Domain model        entities + fields, status sets / state machines
### Rules               every business rule with its number (limits, timers, dedupe)
### Permissions         role × action; where it's enforced
### Interfaces          endpoints or screens, inputs, outputs, error behaviour
### Notifications       who gets what, when, and what never gets sent
### Privacy & security  data handling, retention, abuse protection
### Design              design source, pages and the sections each must have

## Delivery Phases
| Phase | Scope (story #s) | Exit criteria (testable) |

## Testing Decisions
What a good test is; the main seam(s); which rules are written test-first.

## Out of Scope
Everything someone might reasonably assume is included but isn't.

## Open Decisions
| # | Question | Default if undecided |

## Further Notes
What this supersedes, provisional wording, glossary terms still to define.
```

## Filling it

- **Status line:** `ready-for-agent` when the Open Decisions table is empty, otherwise `draft`.
- **Title:** the project name from the crossfire answers, `v1`, and a one-line description of what the app does.
- **Header line:** point `Terms:` at `VOCABULARY.md` at the project root and `Decisions:` at the Decisions section of `system-design.md` (in place of `docs/adr/`), both as paths relative to this file. `Last updated` is today's date.
- **Problem Statement** and **Solution:** from the crossfire answers on purpose, users, and the problem. The Solution names no technology.
- **User Stories:** one story per capability a user has, drawn from the functional requirements and the journeys in `system-design.md`, grouped by actor, numbered across the whole document. Use the actors named in the crossfire answers and the terms from `VOCABULARY.md`. Every functional requirement is covered by at least one story, and every failure path in a journey that the user sees is covered by a story or a rule.
- **Implementation Decisions:**
  - *Architecture:* each component named exactly as in `system-design.md`, with its hosting and environments from the tech stack and the reason from the Decisions.
  - *Domain model:* every entity with its fields from the low-level design, and every status set or state machine with its states and the transitions between them.
  - *Rules:* every business rule with its concrete number (limits, timers, quotas, retries, dedupe windows) as the context states it, each tied to the story numbers it governs.
  - *Permissions:* a role × action table, and the component where each check is enforced.
  - *Interfaces:* every endpoint or screen from the low-level design, with inputs, outputs, and error behavior.
  - *Notifications:* who receives what, on which trigger, through which channel, and what is never sent.
  - *Privacy & security:* data handling, retention, auth, and abuse protection as approved.
  - *Design:* the design source, and each page with the sections it must have.
- **Delivery Phases:** one row per phase from the approved stack, MVP first. Scope lists the story numbers the phase delivers; exit criteria are testable conditions taken from the phase's components and journeys. Every story appears in exactly one phase or in Out of Scope.
- **Testing Decisions:** what a good test is for this app, the main seams (component interfaces from the low-level design), and which rules are written test-first.
- **Out of Scope:** everything the context deferred or excluded, and anything a reader might reasonably assume is included but was not approved.
- **Open Decisions:** every slot written as "Not decided in this session" anywhere in this file, each as a question. The default is the assumption the context states for it, or "Not decided in this session" when it states none.
- **Further Notes:** what this version supersedes (a previous `app-spec.md` in the destination, if one existed), wording still provisional, and `VOCABULARY.md` terms still to define.

Done when every component, entity, functional requirement, journey, and phase from the design files has a place, every story sits in a phase or in Out of Scope, and the Status line matches the Open Decisions table.
