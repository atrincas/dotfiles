# Grill with docs

When the user wants to stress-test a plan, get grilled on a design, or says "grill me", invoke the `mattpocock-skills:grilling` skill together with `mattpocock-skills:domain-modeling` — grill the plan while maintaining the domain model (glossary and ADRs) as decisions crystallise. Prefer this over `grill-me`.

## Presentation

Keep `grilling`'s design tree and frontier logic. Override how it asks:

- Ask **one question per message**, not the whole frontier at once. Frontier questions are independent, so serial order is safe. When the frontier empties, recompute it and continue.
- Ask **closed questions with labeled options** (A, B, C…). Mark your pick **(recommended)**.
- Write questions as markdown in your message — do not use the interactive question picker. The body can run several paragraphs, and the reasoning for your pick sits next to it.
- If a decision has no sensible option set, ask it open rather than inventing options to fill the format.
