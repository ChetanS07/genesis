# system-design.md format

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
