# architecture-overview.html format

Four sections, in this order, behind a sticky section nav:

1. **About**: one paragraph of three to five sentences on what the project is, who it serves, and what it does.
2. **System design**: an inline SVG of the components and their flows, a component table, an inline SVG of the entities and relationships with their cardinality, an entity table (owning component, key attributes, one-line definition) and a relationship table (both entities, cardinality, whether it crosses a component boundary) copied from `system-design.md`, and a short line per journey.
3. **Tech stack**: the phases with their components, then a table of technology picks grouped by phase with layer, technology, hosting model, and reason.
4. **Low-level design and pricing**: the service marks with capability and phase, then one collapsible entry per component, each showing whether it is built or bought, its interfaces and its data: every table or collection it owns with keys, indexes, and constraints, and every reference to another entity (foreign key, join table, or embedded reference, and for cross-component references how consistency is kept), copied from `tech-stack-and-low-level-design.md`; then per service need a tier comparison with the picked option marked, then monthly totals for each phase's picked mix and for each tier.

One self-contained file: inline CSS and JS, no network resources, readable at phone width, light and dark through `prefers-color-scheme`, and clean when printed.

Done when the four sections appear in order and every phase, component name, entity, relationship, table, vendor, price, and total matches the markdown text exactly.
