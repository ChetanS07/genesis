# tech-stack-and-low-level-design.md format

Sections, in order:

1. Phases: each phase with its components, expected users, and monthly budget, MVP first.
2. Tech stack: one row per technology pick, grouped by phase, with the layer (frontend, backend, data, infrastructure, tooling, or any other layer decided), the technology, its hosting model, its version when settled, the reason, the alternative considered, and the lock-in it brings.
3. Low-level design: first the service marks, each with its capability, phase, and why buying beats building; then one entry per component, named exactly as in `system-design.md`, holding its phase, whether it is built or bought, interfaces, data, behavior, runtime, and observability as approved.
4. Vendors and pricing: for each service need, the stack pick or service mark it serves and a table with Starter, Growth, and Scale options. Each option shows the vendor and plan, monthly cost at each phase's expected usage, the limit that forces the next tier, the cost of leaving, the source URL, and the date of the figure. Below the table, the pick for each phase with its reason and the crossfire answer behind it.
5. Monthly totals: each phase's picked mix against that phase's budget, and each tier taken whole, with the arithmetic shown.
6. Assumptions and risks, from both the stack and the low-level design.

Copy figures unchanged from the approved low-level design, and keep the date each was gathered, so the reader can judge how stale it is.

Done when every phase, pick, service mark, component, and service need is present, and every price carries its source and date.
