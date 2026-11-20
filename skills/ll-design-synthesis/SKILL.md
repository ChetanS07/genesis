---
name: ll-design-synthesis
description: Low-level design and vendor selection after stack-synthesis. Fired by genesis, or use when an approved system design and a chosen tech stack are in context and the user wants component-level design or service vendors.
---

# LL design synthesis

Turn the system design and tech stack in context into a low-level design with one vendor chosen per service need, agree it with the user, then hand control back.

## 1. Gather context

Read the crossfire Q&A, the approved system design, and the tech stack decisions in context. When one is missing, fire [genesis](../genesis/SKILL.md) and end this skill; genesis runs the missing stages first and then returns here.

Done when all three are in context.

When a decision needs a fact no answer supplies, or two answers conflict, fire [crossfire](../crossfire/SKILL.md) on that gap, then resume. Whatever you still assume goes under assumptions, marked as yours.

## 2. Design the low level

Give every component in the system design an entry holding:

- Interfaces: endpoints or events, each with request, response, and error shapes
- Data: tables or collections for the entities it owns, named as in the system design, with keys, indexes, and constraints. Each relationship from the system design becomes a foreign key, join table, or embedded reference. A reference to an entity owned by another component states how it is kept consistent (ID only with an API lookup, a replicated copy, or an event).
- Behavior: the sequence for each main journey through it, including failure and retry paths
- Runtime: deployment unit, config, secrets, auth checks
- Observability: the logs, metrics, and alerts that show it has broken

Write each entry in the idioms of the chosen stack.

Done when every component and every main journey from the system design has an entry, and every interface names the entities it reads and writes. Every entity and relationship in the system design maps to a table, collection, key, or reference.

## 3. Choose vendors by tier

Derive the service needs from the design (hosting, database, auth, email, payments, queue, storage, monitoring, and whatever else the components call for). For each need, offer one vendor per tier:

- **Starter**: free or cheapest that works at launch, accepting low limits and manual work
- **Growth**: paid managed service sized for real users
- **Scale**: SLA, compliance certifications, support contract, high limits

Each option states the vendor, its monthly cost at the project's expected usage, the limit that forces the next tier, and the cost of leaving.

Search the vendor's own pages for current pricing and limits before stating them, cite each page, and date the figures. Check that every vendor supports the chosen stack's language and framework.

Set a project tier from the crossfire answers on budget, expected users, and compliance. Pick that tier for each need. Where a need warrants a different tier, say why, citing the answer behind it.

Done when every need has three cited options and one pick whose reason cites a crossfire answer. Run the arithmetic for the monthly total of the picked mix and of each tier taken whole.

## 4. Agree

Present the low-level design, the vendor table with picks, and the monthly totals. Then ask exactly: "Is any change needed, or does it serve your project well?"

When the user asks for changes, apply them and check what else they touch. A vendor swap can change interfaces, and an interface change can change the vendor. Show the revised design with the changes marked, then ask exactly: "Does this serve your project well?" Repeat this round until the user approves.

A yes with a caveat, or "mostly", is a change request. A change that contradicts an earlier crossfire answer goes back to the user as a conflict to settle, with crossfire available for it.

Done when the user's latest message is an explicit yes to the latest version.

## 5. Restate and hand back

Restate the approved version once, under the heading "Approved low-level design": the component entries, the vendor picks with tier and reason, the monthly totals, and the assumptions.

Then return control to the skill that fired this one. When the user fired it directly, fire [genesis](../genesis/SKILL.md), which asks whether to exit or create the files.

Done when the restatement is in context and control is with the firing skill or genesis.
