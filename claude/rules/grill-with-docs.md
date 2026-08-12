# Grill with docs

When the user wants to stress-test a plan, get grilled on a design, or says "grill me", invoke the `mattpocock-skills:grilling` skill together with `mattpocock-skills:domain-modeling` — grill the plan while maintaining the domain model (glossary and ADRs) as decisions crystallise. Prefer this over `grill-me`.

## Presentation

Keep `grilling`'s design tree and frontier logic. Override how it asks:

- Ask **one question per message**, then wait. An answer routinely collapses or reframes questions that looked independent, so a batched round burns questions and makes the user answer things that no longer apply. Recompute the frontier after every answer.
- Ask in **markdown, in your own message** — never `AskUserQuestion` or any other interactive picker. The picker truncates option bodies, so the reasoning that makes an option choosable is exactly what gets lost. This holds in plan mode too: grilling questions are markdown; `AskUserQuestion` and `ExitPlanMode` are only for clarifying scope and plan approval.
- Keep grilling's format — `❓ **Q<n>** — **<title>**`, body, then `➡️` with your pick. Number `<n>` cumulatively across the session; never reset it.
- Offer **labeled options** (**A**, **B**, **C**…) inside that format and mark your pick **(recommended)**, with the reasoning beside it. Say what would change your mind, so the pick is falsifiable rather than decorative.
- If a decision has no sensible option set, ask it open rather than inventing options to fill the format.

## Domain model

Write to `docs/domain/` in the repo being worked on — `glossary.md` for terms, `adr/NNN-<slug>.md` for decisions.

- After an answer that **introduces or redefines a domain term**, add or amend the glossary entry.
- After an answer that **closes a branch with a trade-off** (an option rejected for a stated reason), write an ADR.
- Write as each decision settles, not when the frontier empties, so an interrupted session still leaves a record.
- If `docs/` is gitignored in the repo, say so and ask where the committed home should be rather than writing an untracked trail.
