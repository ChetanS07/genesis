---
name: stack-synthesis
description: Tech stack choice, phased MVP-first, from an approved system design. Fired by genesis, or use when a system design is in context and the user wants technology chosen.
---

# Stack synthesis

Turn the approved system design and the crossfire answers in context into the tech stack of the final product, rolled out in 1 to 3 phases with the MVP as phase 1. Agree it with the user, restate it, then hand control back. The stack lives in the conversation.

Two rules hold in every step:

- **Gaps**: when a decision turns on a fact no answer supplies (team skills, hosting preference, budget, compliance), fire [crossfire](../crossfire/SKILL.md) on that gap, then resume. Whatever you still assume goes under assumptions, marked as yours.
- **Ripple**: every change carries its consequences into the same revision. Recheck everything it touches before showing the result.

## 1. Fix the phases

Read the design and the crossfire Q&A in context. If no approved design is there, fire [genesis](../genesis/SKILL.md) and end this skill; genesis runs the missing stages first and then returns here.

Phase 1 is the MVP: the smallest set of components that serves the core user journeys end to end. Later phases add components and capacity. Phase 1 always meets the **floor**: every core journey runs on phase 1 components alone, and phase 1 holds auth and observability, since both are cheap early and painful to retrofit. When the design lacks auth or observability, add it to phase 1 and mark it as yours.

Draft the phases yourself, then fire [crossfire](../crossfire/SKILL.md) with the draft as the recommended answer. Show every component under its proposed phase with a one-line reason: the journey it serves, why it can wait, or the earlier answer that already placed it. Ask in the same round for the expected users and the monthly budget of each phase, unless the context already holds them. The user accepts with a plain yes or names components to move.

Apply each move with its ripple: a component moved later takes the journeys that need it, and a component moved into phase 1 brings the components it depends on. When a move breaks the floor, say so once and ask again. Show the revised phases with the moves marked until the user accepts.

Done when every component sits in exactly one phase, phase 1 meets the floor, every phase has its expected users and budget (or an assumption marked as yours), and the user has accepted the latest phases.

## 2. Draft the stack

This step selects **technologies**: software the project runs, itself or through a vendor that hosts it (Postgres, Redis, Next.js). Pick one for each layer the design has a component for: language and framework, data stores, hosting and deployment, messaging, observability, CI. Add a technology for a component only when its job is the product's own domain logic. A component with a generic capability (auth, payments, email, search) and every third-party service the design names are left to ll-design-synthesis, which decides whether to build or buy each. Pick for the final product: phase 1 uses a subset and each later phase adds to it, building on what earlier phases shipped.

Each pick carries six lines:

- The technology
- Its hosting model, managed or self-hosted; ll-design-synthesis picks the vendor that runs it under that model
- The phase it enters
- The component or crossfire answer that drove it
- The strongest rejected alternative and why it lost
- The lock-in it brings (what replacing the technology would take), plus its ops burden and learning curve

Read the design's shape, beyond its component list:

- **Data stores** follow the entities and relationships: many-to-many relationships, cross-component references, and the consistency each reference needs decide between relational, document, and key-value storage. Every relationship must be servable by the store picked for its owning component.
- **Messaging** follows the data flow: each asynchronous hop between components needs a queue or broker; a design of synchronous calls alone needs none.
- **Risks**: each risk in the design is answered by a pick, or carried into the stack's risks.

Choose against the user's stated constraints: team skills, timeline, scale, budget per phase, compliance, existing systems. Budget rules on hosting models, phase 1 above all: a managed hosting model in phase 1 must fit the phase 1 budget. Choose _boring_ technology unless an answer demands otherwise, and fewer technologies over more. Look up each pick's current version and maintenance status; memory goes stale.

Name technologies and hosting models; ll-design-synthesis decides build or buy and chooses, plans, and prices every vendor.

Done when every layer the design has a component for, and every domain-logic component needing its own technology, has a pick traced to an answer, every pick carries its six lines, every relationship in the design is servable by its owner's store, every design risk is answered or carried, and every crossfire constraint shapes at least one pick or is listed as not affecting the stack.

## 3. Agree

Present the stack grouped by phase, MVP first, then ask exactly: "Is any change needed, or does this stack serve your project well?"

Apply every requested change with its ripple (a new database moves the ORM, the hosting, the backup plan, and possibly the phases). Show the revised stack with the changes marked, then ask exactly: "Does this serve your project well?" Repeat until the user approves. A yes with a caveat, or "mostly", is a change request.

Settle each change by what it collides with:

- **A real downside**: state it once with the alternative you would pick. A new reason from the user reopens the pick; a plain reaffirmation keeps their choice and moves the downside to risks.
- **A crossfire answer**: put the conflict to the user to settle.
- **The design**: the design needs revising. Say so and fire [system-synthesis](../system-synthesis/SKILL.md); it returns here once the revised design is approved, and the stack is rechecked against it.

Done when the user's latest message is an explicit yes to the latest version and no objection of yours is still open.

## 4. Restate and hand back

Restate the approved version once, under the heading "Approved tech stack": the phases with their components, expected users, and budget; every pick with its six lines, grouped by phase; the assumptions; and the risks.

Then return control to the skill that fired this one. When the user fired it directly, fire [genesis](../genesis/SKILL.md), which continues the flow at the next stage.

Done when the restatement is in context and control is with the firing skill or genesis.
