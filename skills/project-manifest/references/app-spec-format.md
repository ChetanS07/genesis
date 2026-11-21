Status: draft | ready-for-agent
# <App> v1: <one-line description>

Terms: GLOSSARY.md · Decisions: docs/adr/ · Last updated: <date>

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