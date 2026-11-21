# roadmap.md format

A plain-language guide for the humans who run the project: what it is, how it rolls out, and what they must do by hand. Open the file with this note, verbatim, before the title:

```markdown
> **For humans only. Not for agents.**
> This roadmap is a human reference. Agents must not read it, cite it, or use it as a source. Agents read `system-design.md` and `tech-stack-and-low-level-design.md` in this folder instead.
```

Sections, in order:

1. **About the project**: its name, its purpose, its users, and the problem it solves for them, from the crossfire answers, in three to six sentences of plain language.
2. **Phases**: one subsection per phase from the approved stack, MVP first. Each holds:
   - **Delivers**: the components it adds and the journeys that run end to end once it ships, by name as in `system-design.md`.
   - **Expected users and monthly budget**, from the approved stack.
   - **Done when**: a checklist of testable conditions: every component of the phase is live, every journey it delivers runs end to end with its failure paths handled, and auth and observability are in place for phase 1.
   - **Move to the next phase when**: the trigger, taken from the context: the user count or usage that reaches the next phase's expected users, the vendor limit that forces the next tier, or a crossfire answer naming a date or business event. The last phase states what would warrant a phase beyond it, or "Not decided in this session".
3. **MVP**: the features phase 1 delivers, one line each, tied to the core journey it serves; what is deliberately left out and the phase it moves to; and the MVP done criteria: phase 1's "Done when" list plus the success signal from the crossfire answers (first users, a metric, a date), or "Not decided in this session" when none was given.
4. **Human setup between phases**: one subsection before phase 1 and one between each pair of phases, listing everything only a human can do, each as a checklist item with the vendor or system it touches and why it is needed then: accounts to create, plans to buy or upgrade, billing and payment methods, domains and DNS, API keys and secrets to issue and store, access to grant, compliance or legal steps, data migrations or vendor switches flagged in the risks, and any budget approval for the next phase's monthly total.

Write it for a non-technical reader: short sentences, no code, technology and vendor names only where the human acts on them.

Done when every phase, MVP feature, component, and vendor pick that needs a human action appears, every phase has its "Done when" and "Move to the next phase when", and the note opens the file.
