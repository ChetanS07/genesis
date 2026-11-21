---
name: ll-design-synthesis
description: Build-or-buy, low-level design, and vendor selection after stack-synthesis. Fired by genesis, or use when an approved system design and a chosen tech stack are in context and the user wants component-level design or service vendors.
---

# LL design synthesis

Turn the system design and tech stack in context into build-or-buy decisions, a low-level design, and a vendor and plan chosen for each service need in each phase, agree it with the user, then hand control back.

## 1. Gather context

Read the crossfire Q&A, the approved system design, and the section headed "Approved tech stack" in context. When one is missing, fire [genesis](../genesis/SKILL.md) and end this skill; genesis runs the missing stages first and then returns here.

Done when all three are in context.

When a decision needs a fact no answer supplies, or two answers conflict, fire [crossfire](../crossfire/SKILL.md) on that gap, then resume. Whatever you still assume goes under assumptions, marked as yours.

## 2. Build or buy

Decide for each component the stack left open (a generic capability such as auth, payments, email, or search) whether to build it or buy it. Every third-party service the design names is bought.

- **Built**: it runs on the stack's technologies. A library within the stack's language and framework is named in its entry; a component that needs a new technology to run (a data store, a server, a search engine) means the stack needs revising (see step 5).
- **Bought**: give it a **service mark**: the capability the vendor must provide, the phase it enters, and why buying beats building, citing the answers on team skills, timeline, budget, and compliance behind it.

Sort each open component as clear-cut or contested:

- **Clear-cut**: an answer rules one option out (compliance makes building payments unreasonable, the phase 1 budget cannot carry a paid service), or one option wins on every one of team skills, timeline, budget, and compliance. Decide it yourself, with a one-line reason citing the answer behind it.
- **Contested**: both options survive the answers and each wins on at least one of them. Recommend one, then fire [crossfire](../crossfire/SKILL.md) on every contested component as one round, one question each, with your recommendation as the recommended answer. Each question names both options with what each costs (building: effort and upkeep; buying: lock-in and a monthly bill). The user accepts with a plain yes or chooses the other option. Their choice stands: when it carries a real downside, state it once, and a plain reaffirmation keeps their choice with the downside moved to risks.

A bought component in phase 1 must fit the phase 1 budget; when the decided mix breaks it, reopen the component that tips it as contested.

Done when every component is built on the stack or carries a service mark, every mark has its capability, phase, and reason, every clear-cut decision carries its reason, and the user has answered every contested one.

## 3. Design the low level

Give every component in the system design an entry holding:

- Phase: the phase its component enters, from the approved stack
- Source: built, or bought under its service mark. A bought component's entry covers the integration: the calls into the service, the data kept on each side, and the webhooks or events it sends back.
- Interfaces: endpoints or events, each with request, response, and error shapes
- Data: tables or collections for the entities it owns, named as in the system design, with keys, indexes, and constraints. Each relationship from the system design becomes a foreign key, join table, or embedded reference. A reference to an entity owned by another component states how it is kept consistent (ID only with an API lookup, a replicated copy, or an event).
- Behavior: the sequence for each main journey through it, including failure and retry paths
- Runtime: deployment unit, config, secrets, auth checks
- Observability: the logs, metrics, and alerts that show it has broken

Write each entry in the idioms of the chosen stack, on the data store and messaging the stack picked for its component.

Done when every component and every main journey from the system design has an entry, and every interface names the entities it reads and writes. Every entity and relationship in the system design maps to a table, collection, key, or reference.

## 4. Choose vendors by tier

Derive one service need from each stack pick and each service mark:

- **Technology pick**: the need is who runs it, under the stack's hosting model (a managed vendor, or the compute it self-hosts on).
- **Service mark**: the need is the vendor that provides its capability.

A need the components call for with neither a pick nor a mark behind it means the stack needs revising (see step 5).

For each need, offer one option per tier:

- **Starter**: free or cheapest that works at launch, accepting low limits and manual work
- **Growth**: paid managed service sized for real users
- **Scale**: SLA, compliance certifications, support contract, high limits

Each option states the vendor and plan, its monthly cost at each phase's expected users, the limit that forces the next tier, and the cost of leaving the vendor (for a technology, moving it to another host; for a service, replacing the vendor's API and migrating its data).

Search the vendor's own pages for current pricing and limits before stating them, cite each page, and date the figures. Check that every vendor supports the chosen stack's language and framework.

Pick a tier for each phase the need is live in (from the phase its pick or mark enters onward), set by that phase's expected users and budget from the approved stack and by the compliance answers. Where a need warrants a different tier than the rest of its phase, say why, citing the answer behind it. Prefer a vendor whose plans carry the need through every phase, so moving phase is a plan upgrade. When the pick changes vendor between phases, the switch is a migration: list it under risks with its cost of leaving.

Done when every need traces to a stack pick or service mark, has three cited options, and has a pick for each phase it is live in whose reason cites a crossfire answer. Run the arithmetic for the monthly total of each phase's picked mix, and of each tier taken whole at the final phase's usage. A phase total over that phase's budget is flagged when you present it.

## 5. Agree

Present the build-or-buy decisions (the clear-cut ones marked as yours, each with its reason), the low-level design, the vendor table with the picks for each phase, and the monthly totals per phase, flagging any phase over its budget. Then ask exactly: "Is any change needed, or does it serve your project well?"

When the user asks for changes, apply them and check what else they touch. A vendor swap can change interfaces, and an interface change can change the vendor. Show the revised design with the changes marked, then ask exactly: "Does this serve your project well?" Repeat this round until the user approves.

A yes with a caveat, or "mostly", is a change request. A change that contradicts an earlier crossfire answer goes back to the user as a conflict to settle, with crossfire available for it.

A change the stack cannot support (a vendor that needs a different technology or hosting model, a built component that needs a new technology to run, or a need with neither a pick nor a mark behind it) means the stack needs revising: say so and fire [stack-synthesis](../stack-synthesis/SKILL.md). A change the system design cannot support (a new component, entity, or relationship) means the design needs revising: say so and fire [system-synthesis](../system-synthesis/SKILL.md), then stack-synthesis to recheck the stack against it. Each returns here once its revision is approved, and the low-level design and vendor picks are rechecked against it.

Done when the user's latest message is an explicit yes to the latest version.

## 6. Restate and hand back

Restate the approved version once, under the heading "Approved low-level design": the service marks, the component entries, the vendor picks for each phase with tier and reason, the monthly totals per phase, and the assumptions.

Then return control to the skill that fired this one. When the user fired it directly, fire [genesis](../genesis/SKILL.md), which asks whether to exit or create the files.

Done when the restatement is in context and control is with the firing skill or genesis.
