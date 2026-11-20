---
name: system-synthesis
description: System design from crossfire answers. Fired by genesis, or use when requirements are already gathered and the user wants a system design.
---

# System synthesis

Turn the crossfire questions and answers in context into a system design, agree it with the user, then hand control back. The design lives in the conversation.

## 1. Draft the design

Read the crossfire Q&A in context. If none is there, fire [genesis](../genesis/SKILL.md) and end this skill; genesis runs crossfire first and then returns here.

Build the design from the answers. It holds:

- Components, each with one responsibility
- Data flow between components for the main user journeys
- Core data entities, each with its key attributes and the one component that owns it
- Relationships between entities: cardinality (one-to-one, one-to-many, many-to-many), and which entity references which, marking each reference that crosses a component boundary
- External boundaries: users, third-party systems, integrations
- Key decisions, each citing the crossfire answer that drove it
- Risks and assumptions

Name components by role ("job queue", "auth service"). stack-synthesis chooses the products.

Done when every crossfire answer appears in the design or is listed as deliberately left out with a reason, and every component traces to an answer. Every entity has exactly one owner, and every relationship names both entities and its cardinality. Entity names match the terms in `vocabulary.md`.

Every relationship and its cardinality traces to a crossfire answer. Before presenting the design, collect the relationships that trace to no answer and fire [crossfire](../crossfire/SKILL.md) on them as one round, each asked as a concrete scenario with names and numbers. A relationship goes under assumptions, marked as yours, only when the user declines to settle it in that round.

When the design needs a fact no answer supplies, or two answers conflict, fire [crossfire](../crossfire/SKILL.md) on that gap, then resume. Whatever you still assume goes under assumptions, marked as yours.

## 2. Agree

Present the design, then ask exactly: "Is any change needed, or does it serve your project well?"

When the user asks for changes, apply them and check what else they touch. Show the revised design with the changes marked, then ask exactly: "Does this serve your project well?" Repeat this round until the user approves.

A yes with a caveat, or "mostly", is a change request. A change that contradicts an earlier crossfire answer goes back to the user as a conflict to settle, with crossfire available for it.

Done when the user's latest message is an explicit yes to the latest version.

## 3. Hand back

Return control to the skill that fired this one. When the user fired it directly, fire [genesis](../genesis/SKILL.md), which continues the flow at the next stage.

Done when control is with the firing skill or genesis.
